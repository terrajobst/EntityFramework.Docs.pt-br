---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182438"
---
# <a name="model-first"></a><span data-ttu-id="41c19-102">Model First</span><span class="sxs-lookup"><span data-stu-id="41c19-102">Model First</span></span>
<span data-ttu-id="41c19-103">Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Model First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="41c19-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="41c19-104">Model First permite que você crie um novo modelo usando o Entity Framework Designer e, em seguida, gere um esquema de banco de dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="41c19-105">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="41c19-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="41c19-106">As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="41c19-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="41c19-107">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="41c19-107">Watch the video</span></span>
<span data-ttu-id="41c19-108">Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Model First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="41c19-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="41c19-109">Model First permite que você crie um novo modelo usando o Entity Framework Designer e, em seguida, gere um esquema de banco de dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="41c19-110">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="41c19-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="41c19-111">As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="41c19-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="41c19-112">**Apresentado por**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="41c19-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="41c19-113">**Vídeo**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="41c19-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="41c19-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="41c19-114">Pre-Requisites</span></span>

<span data-ttu-id="41c19-115">Você precisará ter o Visual Studio 2010 ou o Visual Studio 2012 instalado para concluir este guia.</span><span class="sxs-lookup"><span data-stu-id="41c19-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="41c19-116">Se você estiver usando o Visual Studio 2010, também será necessário ter o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="41c19-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="41c19-117">1. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="41c19-117">1. Create the Application</span></span>

<span data-ttu-id="41c19-118">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Model First para executar o acesso a dados:</span><span class="sxs-lookup"><span data-stu-id="41c19-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="41c19-119">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41c19-119">Open Visual Studio</span></span>
-   <span data-ttu-id="41c19-120">**Arquivo-&gt; Projeto New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="41c19-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="41c19-121">Selecione **Windows** no menu à esquerda e no **aplicativo de console**</span><span class="sxs-lookup"><span data-stu-id="41c19-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="41c19-122">Insira **ModelFirstSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="41c19-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="41c19-123">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="41c19-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="41c19-124">2. Criar modelo</span><span class="sxs-lookup"><span data-stu-id="41c19-124">2. Create Model</span></span>

<span data-ttu-id="41c19-125">Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="41c19-126">**Projeto-&gt; adicionar novo item...**</span><span class="sxs-lookup"><span data-stu-id="41c19-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="41c19-127">Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="41c19-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="41c19-128">Insira **BloggingModel** como o nome e clique em **OK**, isso iniciará o assistente de modelo de dados de entidade</span><span class="sxs-lookup"><span data-stu-id="41c19-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="41c19-129">Selecione **modelo vazio** e clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="41c19-129">Select **Empty Model** and click **Finish**</span></span>

    ![Criar modelo vazio](~/ef6/media/createemptymodel.png)

<span data-ttu-id="41c19-131">O Entity Framework Designer é aberto com um modelo em branco.</span><span class="sxs-lookup"><span data-stu-id="41c19-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="41c19-132">Agora, podemos começar a adicionar entidades, propriedades e associações ao modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="41c19-133">Clique com o botão direito do mouse na superfície de design e selecione **Propriedades**</span><span class="sxs-lookup"><span data-stu-id="41c19-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="41c19-134">No janela Propriedades alterar o **nome do contêiner de entidade** para **BloggingContext**
    *este é o nome do contexto derivado que será gerado para você, o contexto representa uma sessão com o banco de dados, permitindo que possamos consultar e salvar dados* do</span><span class="sxs-lookup"><span data-stu-id="41c19-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="41c19-135">Clique com o botão direito do mouse na superfície de design e selecione **Adicionar entidade New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="41c19-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="41c19-136">Insira o **blog** como o nome da entidade e o **blogid** como o nome da chave e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="41c19-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Adicionar entidade de blog](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="41c19-138">Clique com o botão direito do mouse na nova entidade na superfície de design e selecione **Adicionar New-&gt; Propriedade escalar**, insira o **nome** como o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="41c19-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="41c19-139">Repita esse processo para adicionar uma propriedade de **URL** .</span><span class="sxs-lookup"><span data-stu-id="41c19-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="41c19-140">Clique com o botão direito do mouse na propriedade **URL** na superfície de design e selecione **propriedades**, na janela Propriedades altere a configuração **Nullable** para **true**
    \*isso nos permite salvar um blog no banco de dados sem atribuir a ele uma URL \*</span><span class="sxs-lookup"><span data-stu-id="41c19-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="41c19-141">Usando as técnicas que você acabou de aprender, adicione uma entidade **post** com uma propriedade de chave **postid**</span><span class="sxs-lookup"><span data-stu-id="41c19-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="41c19-142">Adicionar propriedades escalares de **título** e **conteúdo** à entidade **post**</span><span class="sxs-lookup"><span data-stu-id="41c19-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="41c19-143">Agora que temos algumas entidades, é hora de adicionar uma associação (ou relação) entre elas.</span><span class="sxs-lookup"><span data-stu-id="41c19-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="41c19-144">Clique com o botão direito do mouse na superfície de design e selecione **Adicionar nova associação de &gt;...**</span><span class="sxs-lookup"><span data-stu-id="41c19-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="41c19-145">Faça com que uma extremidade da relação aponte para o **blog** com uma multiplicidade de **um** e o outro ponto de extremidade para **postar** com uma multiplicidade de **muitos**
    *isso significa que um blog tem muitas postagens e uma postagem pertence a um blog*</span><span class="sxs-lookup"><span data-stu-id="41c19-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="41c19-146">Verifique se a caixa de seleção **Adicionar propriedades de chave estrangeira à entidade ' Post '** está marcada e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="41c19-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Adicionar Associação MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="41c19-148">Agora temos um modelo simples para o qual podemos gerar um banco de dados e usá-lo para ler e gravar.</span><span class="sxs-lookup"><span data-stu-id="41c19-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Inicial do modelo](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="41c19-150">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="41c19-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="41c19-151">Se você estiver trabalhando no Visual Studio 2010, há algumas etapas adicionais que você precisa seguir para atualizar para a versão mais recente do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="41c19-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="41c19-152">A atualização é importante porque fornece acesso a uma superfície de API aprimorada, que é muito mais fácil de usar, bem como as correções de bugs mais recentes.</span><span class="sxs-lookup"><span data-stu-id="41c19-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="41c19-153">Primeiro, precisamos obter a versão mais recente do Entity Framework do NuGet.</span><span class="sxs-lookup"><span data-stu-id="41c19-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="41c19-154">**Projeto – &gt; gerenciar pacotes NuGet...** 
    \*se você não tiver a opção **gerenciar pacotes NuGet...** , você deve instalar a [versão mais recente do NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) \*</span><span class="sxs-lookup"><span data-stu-id="41c19-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="41c19-155">Selecione a guia **online**</span><span class="sxs-lookup"><span data-stu-id="41c19-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="41c19-156">Selecionar o pacote do **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="41c19-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="41c19-157">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="41c19-157">Click **Install**</span></span>

<span data-ttu-id="41c19-158">Em seguida, precisamos trocar nosso modelo para gerar código que usa a API DbContext, que foi introduzida em versões posteriores do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="41c19-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="41c19-159">Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="41c19-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="41c19-160">Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**</span><span class="sxs-lookup"><span data-stu-id="41c19-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="41c19-161">Selecione o gerador de DbContext do EF **5. x para C @ no__t-1**, digite **BloggingModel** como o nome e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="41c19-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Modelo DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="41c19-163">3. Gerando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="41c19-163">3. Generating the Database</span></span>

<span data-ttu-id="41c19-164">Considerando nosso modelo, Entity Framework pode calcular um esquema de banco de dados que nos permitirá armazenar e recuperar dados usando o modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="41c19-165">O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:</span><span class="sxs-lookup"><span data-stu-id="41c19-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="41c19-166">Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="41c19-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="41c19-167">Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="41c19-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="41c19-168">Vamos continuar e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="41c19-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="41c19-169">Clique com o botão direito do mouse na superfície de design e selecione **gerar banco de dados do modelo...**</span><span class="sxs-lookup"><span data-stu-id="41c19-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="41c19-170">Clique em **nova conexão...**</span><span class="sxs-lookup"><span data-stu-id="41c19-170">Click **New Connection…**</span></span> <span data-ttu-id="41c19-171">e especifique o LocalDB ou o SQL Express, dependendo da versão do Visual Studio que você está usando, insira **ModelFirst. blog** como o nome do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="41c19-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Conexão LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexão do SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="41c19-174">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="41c19-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="41c19-175">Selecione **Avançar** e o Entity Framework Designer calculará um script para criar o esquema de banco de dados</span><span class="sxs-lookup"><span data-stu-id="41c19-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="41c19-176">Depois que o script for exibido, clique em **concluir** e o script será adicionado ao seu projeto e aberto</span><span class="sxs-lookup"><span data-stu-id="41c19-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="41c19-177">Clique com o botão direito do mouse no script e selecione **executar**. você será solicitado a especificar o banco de dados ao qual se conectar, especificar LocalDB ou SQL Server Express, dependendo da versão do Visual Studio que você está usando</span><span class="sxs-lookup"><span data-stu-id="41c19-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="41c19-178">4. Lendo & gravando dados</span><span class="sxs-lookup"><span data-stu-id="41c19-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="41c19-179">Agora que temos um modelo, é hora de usá-lo para acessar alguns dados.</span><span class="sxs-lookup"><span data-stu-id="41c19-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="41c19-180">As classes que usaremos para acessar os dados estão sendo geradas automaticamente para você com base no arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="41c19-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="41c19-181">*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010, os arquivos BloggingModel.tt e BloggingModel.Context.tt estarão diretamente sob seu projeto, e não aninhados no arquivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="41c19-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Classes geradas](~/ef6/media/generatedclasses.png)

<span data-ttu-id="41c19-183">Implemente o método Main em Program.cs, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="41c19-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="41c19-184">Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo blog.</span><span class="sxs-lookup"><span data-stu-id="41c19-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="41c19-185">Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados ordenado alfabeticamente por título.</span><span class="sxs-lookup"><span data-stu-id="41c19-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="41c19-186">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="41c19-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="41c19-187">5. Lidando com alterações de modelo</span><span class="sxs-lookup"><span data-stu-id="41c19-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="41c19-188">Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações, também precisamos atualizar o esquema do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="41c19-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="41c19-189">Vamos começar adicionando uma nova entidade de usuário ao nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="41c19-190">Adicione um novo nome de entidade de **usuário** com **username** como o nome da chave e a **cadeia de caracteres** como o tipo de propriedade para a chave</span><span class="sxs-lookup"><span data-stu-id="41c19-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Adicionar entidade de usuário](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="41c19-192">Clique com o botão direito do mouse na propriedade **username** na superfície de design e selecione **Properties**, na janela Propriedades altere a configuração **MaxLength** para **50**
    *isso restringe os dados que podem ser armazenados em nome de usuário para 50 caracteres*</span><span class="sxs-lookup"><span data-stu-id="41c19-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="41c19-193">Adicionar uma propriedade escalar **DisplayName** à entidade do **usuário**</span><span class="sxs-lookup"><span data-stu-id="41c19-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="41c19-194">Agora temos um modelo atualizado e estamos prontos para atualizar o banco de dados para acomodar nosso novo tipo de entidade de usuário.</span><span class="sxs-lookup"><span data-stu-id="41c19-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="41c19-195">Clique com o botão direito do mouse na superfície de design e selecione **gerar banco de dados do modelo...** , Entity Framework calculará um script para recriar um esquema com base no modelo atualizado.</span><span class="sxs-lookup"><span data-stu-id="41c19-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="41c19-196">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="41c19-196">Click **Finish**</span></span>
-   <span data-ttu-id="41c19-197">Você pode receber avisos sobre como substituir o script DDL existente e as partes de mapeamento e armazenamento do modelo, clicar em **Sim** para ambos os avisos</span><span class="sxs-lookup"><span data-stu-id="41c19-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="41c19-198">O script SQL atualizado para criar o banco de dados está aberto para você</span><span class="sxs-lookup"><span data-stu-id="41c19-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="41c19-199">o script *The gerado removerá todas as tabelas existentes e, em seguida, recriará o esquema do zero. Isso pode funcionar para o desenvolvimento local, mas não é viável para enviar alterações por push a um banco de dados que já foi implantado. Se você precisar publicar alterações em um banco de dados que já foi implantado, será necessário editar o script ou usar uma ferramenta de comparação de esquema para calcular um script de migração.*</span><span class="sxs-lookup"><span data-stu-id="41c19-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="41c19-200">Clique com o botão direito do mouse no script e selecione **executar**. você será solicitado a especificar o banco de dados ao qual se conectar, especificar LocalDB ou SQL Server Express, dependendo da versão do Visual Studio que você está usando</span><span class="sxs-lookup"><span data-stu-id="41c19-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="41c19-201">Resumo</span><span class="sxs-lookup"><span data-stu-id="41c19-201">Summary</span></span>

<span data-ttu-id="41c19-202">Neste tutorial, examinamos Model First desenvolvimento, que nos permitia criar um modelo no designer do EF e, em seguida, gerar um banco de dados a partir desse modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="41c19-203">Em seguida, usamos o modelo para ler e gravar alguns dados do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="41c19-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="41c19-204">Por fim, atualizamos o modelo e recriamos o esquema de banco de dados para corresponder ao modelo.</span><span class="sxs-lookup"><span data-stu-id="41c19-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
