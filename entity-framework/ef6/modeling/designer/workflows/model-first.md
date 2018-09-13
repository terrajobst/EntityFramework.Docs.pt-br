---
title: Model First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 8e010f95db40261073b4af80a3c0e3225a2cd1cf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490474"
---
# <a name="model-first"></a><span data-ttu-id="df495-102">Model First</span><span class="sxs-lookup"><span data-stu-id="df495-102">Model First</span></span>
<span data-ttu-id="df495-103">Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Model First usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="df495-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="df495-104">Modelo primeiro permite que você criar um novo modelo usando o Entity Framework Designer e, em seguida, gerar um esquema de banco de dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="df495-105">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="df495-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="df495-106">As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="df495-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="df495-107">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="df495-107">Watch the video</span></span>
<span data-ttu-id="df495-108">Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Model First usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="df495-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="df495-109">Modelo primeiro permite que você criar um novo modelo usando o Entity Framework Designer e, em seguida, gerar um esquema de banco de dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="df495-110">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="df495-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="df495-111">As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="df495-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="df495-112">**Apresentado por**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="df495-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="df495-113">**Vídeo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="df495-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="df495-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="df495-114">Pre-Requisites</span></span>

<span data-ttu-id="df495-115">Você precisará ter o Visual Studio 2010 ou Visual Studio 2012 instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="df495-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="df495-116">Se você estiver usando o Visual Studio 2010, você também precisará ter [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="df495-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="df495-117">1. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="df495-117">1. Create the Application</span></span>

<span data-ttu-id="df495-118">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o modelo primeiro para executar acesso a dados:</span><span class="sxs-lookup"><span data-stu-id="df495-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="df495-119">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="df495-119">Open Visual Studio</span></span>
-   <span data-ttu-id="df495-120">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="df495-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="df495-121">Selecione **Windows** no menu à esquerda e **aplicativo de Console**</span><span class="sxs-lookup"><span data-stu-id="df495-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="df495-122">Insira **ModelFirstSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="df495-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="df495-123">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="df495-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="df495-124">2. Criar modelo</span><span class="sxs-lookup"><span data-stu-id="df495-124">2. Create Model</span></span>

<span data-ttu-id="df495-125">Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="df495-126">**Projeto -&gt; Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="df495-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="df495-127">Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="df495-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="df495-128">Insira **BloggingModel** como o nome e clique em **Okey**, isso inicia o Assistente de modelo de dados de entidade</span><span class="sxs-lookup"><span data-stu-id="df495-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="df495-129">Selecione **modelo vazio** e clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="df495-129">Select **Empty Model** and click **Finish**</span></span>

    ![Criar um modelo vazio](~/ef6/media/createemptymodel.png)

<span data-ttu-id="df495-131">O Entity Framework Designer é aberto com um modelo em branco.</span><span class="sxs-lookup"><span data-stu-id="df495-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="df495-132">Agora podemos começar a adicionar entidades, propriedades e associações no modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="df495-133">Clique com botão direito na superfície do design e selecione **propriedades**</span><span class="sxs-lookup"><span data-stu-id="df495-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="df495-134">Em que a alteração da janela de propriedades de **nome do contêiner de entidade** para **BloggingContext**
    *esse é o nome do contexto derivado que será gerado para você, o contexto representa uma sessão com o banco de dados, que nos permite consultar e salvar dados*</span><span class="sxs-lookup"><span data-stu-id="df495-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="df495-135">Clique com botão direito na superfície do design e selecione **adicionar novo -&gt; Entity...**</span><span class="sxs-lookup"><span data-stu-id="df495-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="df495-136">Insira **Blog** como o nome da entidade e **BlogId** como o nome da chave e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="df495-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Adicionar entidade de Blog](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="df495-138">Clique com botão direito na nova entidade na superfície do design e selecione **adicionar novo -&gt; propriedade escalar**, insira **nome** como o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="df495-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="df495-139">Repita esse processo para adicionar um **Url** propriedade.</span><span class="sxs-lookup"><span data-stu-id="df495-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="df495-140">Com o botão direito no **Url** propriedade na superfície do design e selecione **propriedades**, em que a alteração da janela de propriedades a **Nullable** definindo como **True** 
     *Isso nos permite economizar um Blog no banco de dados sem atribuí-lo uma Url*</span><span class="sxs-lookup"><span data-stu-id="df495-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="df495-141">Usando as técnicas que você acabou de adquiridos, adicione uma **Post** entidade com um **PostId** propriedade de chave</span><span class="sxs-lookup"><span data-stu-id="df495-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="df495-142">Adicione **Title** e **conteúdo** propriedades escalares para o **Post** entidade</span><span class="sxs-lookup"><span data-stu-id="df495-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="df495-143">Agora que temos duas entidades, é hora de adicionar uma associação (ou relação) entre eles.</span><span class="sxs-lookup"><span data-stu-id="df495-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="df495-144">Clique com botão direito na superfície do design e selecione **adicionar novo -&gt; associação...**</span><span class="sxs-lookup"><span data-stu-id="df495-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="df495-145">Fazer uma extremidade da relação aponte para **Blog** com uma multiplicidade de **um** e o outro ponto de extremidade para **Post** com uma multiplicidade de **muitos** 
     *Isso significa que um Blog tem muitas postagens e uma postagem pertencer a um Blog*</span><span class="sxs-lookup"><span data-stu-id="df495-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="df495-146">Verifique se o **adicionar propriedades de chave estrangeira para 'Post' Entity** caixa está marcada e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="df495-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Adicionar associação MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="df495-148">Agora temos um modelo simples que podemos gerar um banco de dados e usar para ler e gravar dados.</span><span class="sxs-lookup"><span data-stu-id="df495-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Modelo inicial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="df495-150">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="df495-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="df495-151">Se você estiver trabalhando no Visual Studio 2010, há algumas etapas adicionais que você precisa seguir para atualizar para a versão mais recente do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="df495-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="df495-152">A atualização é importante porque ele fornece acesso a uma superfície de API aprimorada, que é muito mais fácil de usar, bem como as correções de bug mais recentes.</span><span class="sxs-lookup"><span data-stu-id="df495-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="df495-153">Primeiro, precisamos obter a versão mais recente do Entity Framework do NuGet.</span><span class="sxs-lookup"><span data-stu-id="df495-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="df495-154">\*\*Projeto –&gt; gerenciar pacotes NuGet... \*\* 
     \*Se você não tiver o \**gerenciar pacotes NuGet... \*\* opção, você deve instalar o [versão mais recente do NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="df495-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="df495-155">Selecione o **Online** guia</span><span class="sxs-lookup"><span data-stu-id="df495-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="df495-156">Selecione o **EntityFramework** pacote</span><span class="sxs-lookup"><span data-stu-id="df495-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="df495-157">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="df495-157">Click **Install**</span></span>

<span data-ttu-id="df495-158">Em seguida, precisamos alternar nosso modelo para gerar o código que usa a API DbContext, que foi introduzida em versões posteriores do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="df495-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="df495-159">Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="df495-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="df495-160">Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**</span><span class="sxs-lookup"><span data-stu-id="df495-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="df495-161">Selecione o EF **5.x gerador DbContext para C\#**, insira **BloggingModel** como o nome e clique em **adicionar**</span><span class="sxs-lookup"><span data-stu-id="df495-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Modelo de DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="df495-163">3. Gerando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="df495-163">3. Generating the Database</span></span>

<span data-ttu-id="df495-164">Considerando nosso modelo, o Entity Framework pode calcular um esquema de banco de dados que nos permitirá armazenar e recuperar dados usando o modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="df495-165">O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="df495-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="df495-166">Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="df495-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="df495-167">Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="df495-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="df495-168">Vamos prosseguir e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="df495-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="df495-169">Clique com botão direito na superfície do design e selecione **gerar banco de dados do modelo...**</span><span class="sxs-lookup"><span data-stu-id="df495-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="df495-170">Clique em **nova Conexão...**</span><span class="sxs-lookup"><span data-stu-id="df495-170">Click **New Connection…**</span></span> <span data-ttu-id="df495-171">e especifique o LocalDB ou Express do SQL, dependendo de qual versão do Visual Studio, você está usando, insira **ModelFirst.Blogging** como o nome do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="df495-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Conexão LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexão do SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="df495-174">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="df495-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="df495-175">Selecione **próxima** e o Entity Framework Designer calculará um script para criar o esquema de banco de dados</span><span class="sxs-lookup"><span data-stu-id="df495-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="df495-176">Depois que o script é exibido, clique em **concluir** e o script será adicionado ao seu projeto e aberto</span><span class="sxs-lookup"><span data-stu-id="df495-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="df495-177">Clique duas vezes em script e selecione **Execute**, você será solicitado a especificar o banco de dados para se conectar ao, especifique o LocalDB ou Express do SQL Server, dependendo de qual versão do Visual Studio que você está usando</span><span class="sxs-lookup"><span data-stu-id="df495-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="df495-178">4. Ler e gravar dados</span><span class="sxs-lookup"><span data-stu-id="df495-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="df495-179">Agora que temos um modelo que é hora de usá-lo para acessar alguns dados.</span><span class="sxs-lookup"><span data-stu-id="df495-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="df495-180">As classes que vamos usar para acessar os dados são gerados automaticamente para você com base no arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="df495-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="df495-181">*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010 a BloggingModel.tt e BloggingModel.Context.tt arquivos diretamente em seu projeto, em vez de aninhado sob o arquivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="df495-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Classes geradas](~/ef6/media/generatedclasses.png)

<span data-ttu-id="df495-183">Implemente o método Main em Program.cs, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="df495-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="df495-184">Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir um novo Blog.</span><span class="sxs-lookup"><span data-stu-id="df495-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="df495-185">Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados classificado em ordem alfabética por título.</span><span class="sxs-lookup"><span data-stu-id="df495-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="df495-186">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="df495-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="df495-187">5. Lidando com alterações no modelo</span><span class="sxs-lookup"><span data-stu-id="df495-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="df495-188">Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações que também precisamos atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="df495-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="df495-189">Vamos começar adicionando uma nova entidade de usuário ao nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="df495-190">Adicione um novo **usuário** com o nome da entidade **nome de usuário** como o nome da chave e **cadeia de caracteres** como o tipo de propriedade da chave</span><span class="sxs-lookup"><span data-stu-id="df495-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Adicionar entidade de usuário](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="df495-192">Com o botão direito no **nome de usuário** propriedade na superfície do design e selecione **propriedades**, alteração de janela em Propriedades de **MaxLength** definindo como \*\*50 \*\* 
     *Isso restringe os dados que podem ser armazenados em nome de usuário a 50 caracteres*</span><span class="sxs-lookup"><span data-stu-id="df495-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="df495-193">Adicionar um **DisplayName** propriedade escalar para o **usuário** entidade</span><span class="sxs-lookup"><span data-stu-id="df495-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="df495-194">Agora temos um modelo atualizado e você está pronto para atualizar o banco de dados para acomodar o nosso novo tipo de entidade do usuário.</span><span class="sxs-lookup"><span data-stu-id="df495-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="df495-195">Clique com botão direito na superfície do design e selecione **gerar banco de dados do modelo...** , Entity Framework irá calcular um script para recriar um esquema com base no modelo atualizado.</span><span class="sxs-lookup"><span data-stu-id="df495-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="df495-196">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="df495-196">Click **Finish**</span></span>
-   <span data-ttu-id="df495-197">Você pode receber avisos sobre a substituição de script DDL existente e as partes de armazenamento e mapeamento do modelo, clique em **Sim** para ambos os esses avisos</span><span class="sxs-lookup"><span data-stu-id="df495-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="df495-198">O script SQL atualizado para criar o banco de dados é aberto para você</span><span class="sxs-lookup"><span data-stu-id="df495-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="df495-199">*O script gerado será descartar todas as tabelas existentes e, em seguida, recriar o esquema do zero. Isso pode funcionar para o desenvolvimento local, mas não é viável para enviar alterações por push para um banco de dados que já foi implantado. Se você precisar publicar alterações em um banco de dados que já tenha sido implantado, você precisará editar o script ou usar uma ferramenta de comparação de esquema para calcular um script de migração.*</span><span class="sxs-lookup"><span data-stu-id="df495-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="df495-200">Clique duas vezes em script e selecione **Execute**, você será solicitado a especificar o banco de dados para se conectar ao, especifique o LocalDB ou Express do SQL Server, dependendo de qual versão do Visual Studio que você está usando</span><span class="sxs-lookup"><span data-stu-id="df495-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="df495-201">Resumo</span><span class="sxs-lookup"><span data-stu-id="df495-201">Summary</span></span>

<span data-ttu-id="df495-202">Neste passo a passo que vimos desenvolvimento Model First, que nos permitiu criar um modelo no Designer de EF e, em seguida, gerar um banco de dados de modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="df495-203">Em seguida, usamos o modelo para ler e gravar alguns dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="df495-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="df495-204">Finalmente, atualizado o modelo e, em seguida, recriado o esquema de banco de dados para corresponder ao modelo.</span><span class="sxs-lookup"><span data-stu-id="df495-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
