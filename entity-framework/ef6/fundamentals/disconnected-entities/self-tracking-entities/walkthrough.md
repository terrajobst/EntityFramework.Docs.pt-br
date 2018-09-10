---
title: Passo a passo de entidades - EF6 de autocontrole
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 1c450bbb20c246d9b9d58707ac03eb48eadfa970
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251278"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="016a6-102">Passo a passo entidades de autocontrole</span><span class="sxs-lookup"><span data-stu-id="016a6-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="016a6-103">Não recomendamos usar o modelo de entidades de rastreamento automático.</span><span class="sxs-lookup"><span data-stu-id="016a6-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="016a6-104">Ele continuará disponível apenas para dar suporte aos aplicativos existentes.</span><span class="sxs-lookup"><span data-stu-id="016a6-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="016a6-105">Se o seu aplicativo exigir o trabalho com grafos de entidades desconectados, considere outras alternativas como [Entidades Rastreáveis](http://trackableentities.github.io/), que são uma tecnologia semelhante às Entidades de Rastreamento Automático desenvolvidas mais ativamente pela comunidade, ou escreva um código personalizado usando as APIs de controle de alterações de baixo nível.</span><span class="sxs-lookup"><span data-stu-id="016a6-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="016a6-106">Este passo a passo demonstra o cenário no qual um serviço do Windows Communication Foundation (WCF) expõe uma operação que retorna um gráfico de entidade.</span><span class="sxs-lookup"><span data-stu-id="016a6-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="016a6-107">Em seguida, um aplicativo cliente manipula esse gráfico e envia as modificações em uma operação de serviço que valida e salva as atualizações em um banco de dados usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="016a6-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="016a6-108">Antes de concluir este passo a passo, não deixe de ler o [entidades de autorrastreamento](index.md) página.</span><span class="sxs-lookup"><span data-stu-id="016a6-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="016a6-109">Este passo a passo conclui as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="016a6-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="016a6-110">Cria um banco de dados para acessar.</span><span class="sxs-lookup"><span data-stu-id="016a6-110">Creates a database to access.</span></span>
-   <span data-ttu-id="016a6-111">Cria uma biblioteca de classes que contém o modelo.</span><span class="sxs-lookup"><span data-stu-id="016a6-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="016a6-112">Trocas do modelo de rastreamento automático gerador de entidade.</span><span class="sxs-lookup"><span data-stu-id="016a6-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="016a6-113">Move as classes de entidade para um projeto separado.</span><span class="sxs-lookup"><span data-stu-id="016a6-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="016a6-114">Cria um serviço WCF que expõe as operações para consultar e salvar as entidades.</span><span class="sxs-lookup"><span data-stu-id="016a6-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="016a6-115">Cria aplicativos (Console e WPF) que consomem o serviço de cliente.</span><span class="sxs-lookup"><span data-stu-id="016a6-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="016a6-116">Vamos usar banco de dados primeiro neste passo a passo, mas as mesmas técnicas se aplicam igualmente ao Model First.</span><span class="sxs-lookup"><span data-stu-id="016a6-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="016a6-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="016a6-117">Pre-Requisites</span></span>

<span data-ttu-id="016a6-118">Para concluir este passo a passo, você precisará uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="016a6-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="016a6-119">Criar um banco de dados</span><span class="sxs-lookup"><span data-stu-id="016a6-119">Create a Database</span></span>

<span data-ttu-id="016a6-120">O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="016a6-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="016a6-121">Se você estiver usando o Visual Studio 2012, em seguida, você criará um banco de dados LocalDB.</span><span class="sxs-lookup"><span data-stu-id="016a6-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="016a6-122">Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="016a6-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="016a6-123">Vamos prosseguir e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="016a6-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="016a6-124">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="016a6-124">Open Visual Studio</span></span>
-   <span data-ttu-id="016a6-125">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="016a6-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="016a6-126">Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="016a6-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="016a6-127">Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar **Microsoft SQL Server** como fonte de dados</span><span class="sxs-lookup"><span data-stu-id="016a6-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="016a6-128">Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado</span><span class="sxs-lookup"><span data-stu-id="016a6-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="016a6-129">Insira **STESample** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="016a6-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="016a6-130">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="016a6-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="016a6-131">O novo banco de dados agora aparecerão no Gerenciador de servidores</span><span class="sxs-lookup"><span data-stu-id="016a6-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="016a6-132">Se você estiver usando o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="016a6-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="016a6-133">Clique com botão direito no banco de dados no Gerenciador de servidores e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="016a6-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="016a6-134">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="016a6-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="016a6-135">Se você estiver usando o Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="016a6-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="016a6-136">Selecione **dados -&gt; Transact SQL Editor -&gt; nova Conexão de consulta...**</span><span class="sxs-lookup"><span data-stu-id="016a6-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="016a6-137">Insira **.\\ SQLEXPRESS** como o nome do servidor e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="016a6-138">Selecione o **STESample** de banco de dados na lista suspensa na parte superior do editor de consultas</span><span class="sxs-lookup"><span data-stu-id="016a6-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="016a6-139">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **executar SQL**</span><span class="sxs-lookup"><span data-stu-id="016a6-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="016a6-140">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="016a6-140">Create the Model</span></span>

<span data-ttu-id="016a6-141">Primeiro, precisamos colocar o modelo em um projeto.</span><span class="sxs-lookup"><span data-stu-id="016a6-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="016a6-142">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="016a6-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="016a6-143">Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **biblioteca de classes**</span><span class="sxs-lookup"><span data-stu-id="016a6-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="016a6-144">Insira **STESample** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="016a6-145">Agora vamos criar um modelo simples no EF Designer para acessar nosso banco de dados:</span><span class="sxs-lookup"><span data-stu-id="016a6-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="016a6-146">**Projeto -&gt; Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="016a6-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="016a6-147">Selecione **dados** no painel esquerdo e, em seguida, **modelo de dados de entidade ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="016a6-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="016a6-148">Insira **BloggingModel** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="016a6-149">Selecione **gerar a partir do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="016a6-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="016a6-150">Insira as informações de conexão para o banco de dados que você criou na seção anterior</span><span class="sxs-lookup"><span data-stu-id="016a6-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="016a6-151">Insira **BloggingContext** como o nome para a cadeia de caracteres de conexão e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="016a6-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="016a6-152">A caixa de seleção ao lado **tabelas** e clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="016a6-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="016a6-153">Alterne para a geração de código lar</span><span class="sxs-lookup"><span data-stu-id="016a6-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="016a6-154">Agora, é necessário desabilitar a geração de código padrão e a troca para entidades de autorrastreamento.</span><span class="sxs-lookup"><span data-stu-id="016a6-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="016a6-155">Se você estiver usando o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="016a6-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="016a6-156">Expandir **BloggingModel.edmx** na **Gerenciador de soluções** e exclua os **BloggingModel.tt** e **BloggingModel.Context.tt** 
     *Isso desabilitará a geração de código padrão*</span><span class="sxs-lookup"><span data-stu-id="016a6-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="016a6-157">Clique com botão direito no EF Designer na superfície e selecione uma área vazia **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="016a6-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="016a6-158">Selecione **Online** no painel esquerdo e procure para **gerador lar**</span><span class="sxs-lookup"><span data-stu-id="016a6-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="016a6-159">Selecione o **lar gerador para C\#**  modelo, insira **STETemplate** como o nome e clique em **adicionar**</span><span class="sxs-lookup"><span data-stu-id="016a6-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="016a6-160">O **STETemplate.tt** e **STETemplate.Context.tt** arquivos são adicionados aninhado sob o arquivo BloggingModel.edmx</span><span class="sxs-lookup"><span data-stu-id="016a6-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="016a6-161">Se você estiver usando o Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="016a6-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="016a6-162">Clique com botão direito no EF Designer na superfície e selecione uma área vazia **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="016a6-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="016a6-163">Selecione **código** no painel esquerdo e, em seguida, **gerador de entidade de rastreamento automático do ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="016a6-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="016a6-164">Insira **STETemplate** como o nome e clique em **adicionar**</span><span class="sxs-lookup"><span data-stu-id="016a6-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="016a6-165">O **STETemplate.tt** e **STETemplate.Context.tt** arquivos são adicionados diretamente ao seu projeto</span><span class="sxs-lookup"><span data-stu-id="016a6-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="016a6-166">Mover os tipos de entidade em um projeto separado</span><span class="sxs-lookup"><span data-stu-id="016a6-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="016a6-167">Para usar entidades de autorrastreamento nosso aplicativo cliente precisa acessar as classes de entidade gerados a partir de nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="016a6-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="016a6-168">Porque não queremos expor o modelo inteiro para o aplicativo cliente, vamos para mover as classes de entidade em um projeto separado.</span><span class="sxs-lookup"><span data-stu-id="016a6-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="016a6-169">A primeira etapa é pare de gerar classes de entidade no projeto existente:</span><span class="sxs-lookup"><span data-stu-id="016a6-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="016a6-170">Clique duas vezes em **STETemplate.tt** na **Gerenciador de soluções** e selecione **propriedades**</span><span class="sxs-lookup"><span data-stu-id="016a6-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="016a6-171">No **propriedades** janela clara **TextTemplatingFileGenerator** do **CustomTool** propriedade</span><span class="sxs-lookup"><span data-stu-id="016a6-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="016a6-172">Expandir **STETemplate.tt** na **Gerenciador de soluções** e excluir todos os arquivos aninhados sob ela</span><span class="sxs-lookup"><span data-stu-id="016a6-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="016a6-173">Em seguida, vamos adicionar um novo projeto e gerar as classes de entidade nele</span><span class="sxs-lookup"><span data-stu-id="016a6-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="016a6-174">**Arquivo -&gt; Add -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="016a6-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="016a6-175">Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **biblioteca de classes**</span><span class="sxs-lookup"><span data-stu-id="016a6-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="016a6-176">Insira **STESample.Entities** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="016a6-177">**Projeto -&gt; Adicionar Item existente...**</span><span class="sxs-lookup"><span data-stu-id="016a6-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="016a6-178">Navegue até a **STESample** pasta do projeto</span><span class="sxs-lookup"><span data-stu-id="016a6-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="016a6-179">Selecione para exibir **todos os arquivos (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="016a6-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="016a6-180">Selecione o **STETemplate.tt** arquivo</span><span class="sxs-lookup"><span data-stu-id="016a6-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="016a6-181">Clique na seta suspensa ao lado de **Add** botão e selecione **adicionar como vínculo**</span><span class="sxs-lookup"><span data-stu-id="016a6-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Adicionar modelo vinculado](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="016a6-183">Também vamos para garantir que as classes de entidade obterem geradas no mesmo namespace que o contexto.</span><span class="sxs-lookup"><span data-stu-id="016a6-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="016a6-184">Isso apenas reduz o número de instruções, precisamos adicionar em todo o nosso aplicativo de uso.</span><span class="sxs-lookup"><span data-stu-id="016a6-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="016a6-185">Clique duas vezes em vinculada **STETemplate.tt** na **Gerenciador de soluções** e selecione **propriedades**</span><span class="sxs-lookup"><span data-stu-id="016a6-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="016a6-186">No **propriedades** janela conjunto **Namespace de ferramenta personalizada** para **STESample**</span><span class="sxs-lookup"><span data-stu-id="016a6-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="016a6-187">O código gerado pelo modelo lar precisará de uma referência a **Serialization** fim de compilar.</span><span class="sxs-lookup"><span data-stu-id="016a6-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="016a6-188">Essa biblioteca é necessária para o WCF **DataContract** e **DataMember** atributos que são usados nos tipos de entidade serializável.</span><span class="sxs-lookup"><span data-stu-id="016a6-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="016a6-189">Clique com botão direito do **STESample.Entities** project no **Gerenciador de soluções** e selecione **adicionar referência...**</span><span class="sxs-lookup"><span data-stu-id="016a6-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="016a6-190">No Visual Studio 2012 – marque a caixa ao lado **Serialization** e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="016a6-191">No Visual Studio 2010 - selecione **Serialization** e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="016a6-192">Por fim, o projeto com o nosso contexto em que ele precisa de uma referência para os tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="016a6-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="016a6-193">Clique com botão direito do **STESample** project no **Gerenciador de soluções** e selecione **adicionar referência...**</span><span class="sxs-lookup"><span data-stu-id="016a6-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="016a6-194">No Visual Studio 2012 – selecione **Solution** no painel esquerdo, marque a caixa ao lado de **STESample.Entities** e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="016a6-195">No Visual Studio 2010 – selecione o **projetos** guia, selecione **STESample.Entities** e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="016a6-196">Outra opção para mover os tipos de entidade para um projeto separado é mover o arquivo de modelo, em vez de vinculá-lo do local padrão.</span><span class="sxs-lookup"><span data-stu-id="016a6-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="016a6-197">Se você fizer isso, você precisará atualizar o **arquivo_de_entrada** variável no modelo para fornecer o caminho relativo para o arquivo edmx (neste exemplo que seriam **... \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="016a6-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="016a6-198">Criar um serviço WCF</span><span class="sxs-lookup"><span data-stu-id="016a6-198">Create a WCF Service</span></span>

<span data-ttu-id="016a6-199">Agora é hora de adicionar um serviço WCF para expor nossos dados, vamos começar pela criação do projeto.</span><span class="sxs-lookup"><span data-stu-id="016a6-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="016a6-200">**Arquivo -&gt; Add -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="016a6-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="016a6-201">Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **aplicativo de serviço WCF**</span><span class="sxs-lookup"><span data-stu-id="016a6-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="016a6-202">Insira **STESample.Service** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="016a6-203">Adicione uma referência para o **Entity** assembly</span><span class="sxs-lookup"><span data-stu-id="016a6-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="016a6-204">Adicione uma referência para o **STESample** e **STESample.Entities** projetos</span><span class="sxs-lookup"><span data-stu-id="016a6-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="016a6-205">É preciso copiar a cadeia de caracteres de conexão do EF para este projeto para que ele é encontrado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="016a6-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="016a6-206">Abra o **App. config** do arquivo para o \* \* STESample \* \* projeto e copie o **connectionStrings** elemento</span><span class="sxs-lookup"><span data-stu-id="016a6-206">Open the **App.Config** file for the \*\*STESample \*\*project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="016a6-207">Colar a **connectionStrings** como um elemento filho da **configuration** elemento do **Web. config** de arquivo no **STESample.Service** projeto</span><span class="sxs-lookup"><span data-stu-id="016a6-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="016a6-208">Agora é hora de implementar o serviço real.</span><span class="sxs-lookup"><span data-stu-id="016a6-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="016a6-209">Abra **IService1.cs** e substitua o conteúdo com o código a seguir</span><span class="sxs-lookup"><span data-stu-id="016a6-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="016a6-210">Abra **Service1.svc** e substitua o conteúdo com o código a seguir</span><span class="sxs-lookup"><span data-stu-id="016a6-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="016a6-211">Consumir o serviço de um aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="016a6-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="016a6-212">Vamos criar um aplicativo de console que usa o nosso serviço.</span><span class="sxs-lookup"><span data-stu-id="016a6-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="016a6-213">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="016a6-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="016a6-214">Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **aplicativo de Console**</span><span class="sxs-lookup"><span data-stu-id="016a6-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="016a6-215">Insira **STESample.ConsoleTest** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="016a6-216">Adicione uma referência para o **STESample.Entities** projeto</span><span class="sxs-lookup"><span data-stu-id="016a6-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="016a6-217">Precisamos de uma referência de serviço para nosso serviço WCF</span><span class="sxs-lookup"><span data-stu-id="016a6-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="016a6-218">Clique com botão direito do **STESample.ConsoleTest** project no **Gerenciador de soluções** e selecione **adicionar referência de serviço...**</span><span class="sxs-lookup"><span data-stu-id="016a6-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="016a6-219">Clique em **descobrir**</span><span class="sxs-lookup"><span data-stu-id="016a6-219">Click **Discover**</span></span>
-   <span data-ttu-id="016a6-220">Insira **BloggingService** como o namespace e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="016a6-221">Agora podemos escrever algum código para consumir o serviço.</span><span class="sxs-lookup"><span data-stu-id="016a6-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="016a6-222">Abra **Program.cs** e substitua o conteúdo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="016a6-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="016a6-223">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="016a6-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="016a6-224">Clique com botão direito do **STESample.ConsoleTest** project no **Gerenciador de soluções** e selecione **Debug -&gt; iniciar nova instância**</span><span class="sxs-lookup"><span data-stu-id="016a6-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="016a6-225">Você verá a seguinte saída quando o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="016a6-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="016a6-226">Consumir o serviço de um aplicativo WPF</span><span class="sxs-lookup"><span data-stu-id="016a6-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="016a6-227">Vamos criar um aplicativo WPF que usa o nosso serviço.</span><span class="sxs-lookup"><span data-stu-id="016a6-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="016a6-228">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="016a6-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="016a6-229">Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **aplicativo WPF**</span><span class="sxs-lookup"><span data-stu-id="016a6-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="016a6-230">Insira **STESample.WPFTest** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="016a6-231">Adicione uma referência para o **STESample.Entities** projeto</span><span class="sxs-lookup"><span data-stu-id="016a6-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="016a6-232">Precisamos de uma referência de serviço para nosso serviço WCF</span><span class="sxs-lookup"><span data-stu-id="016a6-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="016a6-233">Clique com botão direito do **STESample.WPFTest** project no **Gerenciador de soluções** e selecione **adicionar referência de serviço...**</span><span class="sxs-lookup"><span data-stu-id="016a6-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="016a6-234">Clique em **descobrir**</span><span class="sxs-lookup"><span data-stu-id="016a6-234">Click **Discover**</span></span>
-   <span data-ttu-id="016a6-235">Insira **BloggingService** como o namespace e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="016a6-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="016a6-236">Agora podemos escrever algum código para consumir o serviço.</span><span class="sxs-lookup"><span data-stu-id="016a6-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="016a6-237">Abra **MainWindow. XAML** e substitua o conteúdo com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="016a6-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="016a6-238">Abra o code-behind para MainWindow (**MainWindow.xaml.cs**) e substitua o conteúdo com o código a seguir</span><span class="sxs-lookup"><span data-stu-id="016a6-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="016a6-239">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="016a6-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="016a6-240">Clique com botão direito do **STESample.WPFTest** project no **Gerenciador de soluções** e selecione **Debug -&gt; iniciar nova instância**</span><span class="sxs-lookup"><span data-stu-id="016a6-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="016a6-241">Você pode manipular os dados usando a tela e salvá-lo por meio do serviço usando o **salvar** botão</span><span class="sxs-lookup"><span data-stu-id="016a6-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![Janela principal do WPF](~/ef6/media/wpf.png)
