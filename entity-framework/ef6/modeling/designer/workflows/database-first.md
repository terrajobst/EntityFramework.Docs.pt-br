---
title: Database First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182456"
---
# <a name="database-first"></a><span data-ttu-id="363c3-102">Database First</span><span class="sxs-lookup"><span data-stu-id="363c3-102">Database First</span></span>
<span data-ttu-id="363c3-103">Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Database First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="363c3-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="363c3-104">Database First permite que você reverta a engenharia de um modelo de um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="363c3-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="363c3-105">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="363c3-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="363c3-106">As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="363c3-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="363c3-107">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="363c3-107">Watch the video</span></span>
<span data-ttu-id="363c3-108">Este vídeo fornece uma introdução ao desenvolvimento de Database First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="363c3-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="363c3-109">Database First permite que você reverta a engenharia de um modelo de um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="363c3-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="363c3-110">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="363c3-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="363c3-111">As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="363c3-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="363c3-112">**Apresentado por**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="363c3-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="363c3-113">**Vídeo**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="363c3-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="363c3-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="363c3-114">Pre-Requisites</span></span>

<span data-ttu-id="363c3-115">Você precisará ter, pelo menos, o Visual Studio 2010 ou o Visual Studio 2012 instalado para concluir esta explicação.</span><span class="sxs-lookup"><span data-stu-id="363c3-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="363c3-116">Se você estiver usando o Visual Studio 2010, também será necessário ter o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="363c3-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="363c3-117">1. Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="363c3-117">1. Create an Existing Database</span></span>

<span data-ttu-id="363c3-118">Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.</span><span class="sxs-lookup"><span data-stu-id="363c3-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="363c3-119">O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:</span><span class="sxs-lookup"><span data-stu-id="363c3-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="363c3-120">Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="363c3-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="363c3-121">Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="363c3-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="363c3-122">Vamos continuar e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="363c3-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="363c3-123">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363c3-123">Open Visual Studio</span></span>
-   <span data-ttu-id="363c3-124">**Exibir-&gt; Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="363c3-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="363c3-125">Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**</span><span class="sxs-lookup"><span data-stu-id="363c3-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="363c3-126">Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado</span><span class="sxs-lookup"><span data-stu-id="363c3-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="363c3-128">Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou e insira **DatabaseFirst. blog** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="363c3-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![DF de conexão do SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![DF de conexão LocalDB](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="363c3-131">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="363c3-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Caixa de diálogo Criar banco de dados](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="363c3-133">O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="363c3-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="363c3-134">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="363c3-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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
```

## <a name="2-create-the-application"></a><span data-ttu-id="363c3-135">2. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="363c3-135">2. Create the Application</span></span>

<span data-ttu-id="363c3-136">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Database First para executar o acesso a dados:</span><span class="sxs-lookup"><span data-stu-id="363c3-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="363c3-137">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363c3-137">Open Visual Studio</span></span>
-   <span data-ttu-id="363c3-138">**Arquivo-&gt; Projeto New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="363c3-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="363c3-139">Selecione **Windows** no menu à esquerda e no **aplicativo de console**</span><span class="sxs-lookup"><span data-stu-id="363c3-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="363c3-140">Insira **DatabaseFirstSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="363c3-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="363c3-141">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="363c3-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="363c3-142">3. Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="363c3-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="363c3-143">Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="363c3-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="363c3-144">**Projeto-&gt; adicionar novo item...**</span><span class="sxs-lookup"><span data-stu-id="363c3-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="363c3-145">Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="363c3-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="363c3-146">Insira **BloggingModel** como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="363c3-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="363c3-147">Isso iniciará o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="363c3-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="363c3-148">Selecione **gerar do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="363c3-148">Select **Generate from Database** and click **Next**</span></span>

    ![Etapa 1 do assistente](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="363c3-150">Selecione a conexão com o banco de dados que você criou na primeira seção, digite **BloggingContext** como o nome da cadeia de conexão e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="363c3-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Etapa 2 do assistente](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="363c3-152">Clique na caixa de seleção ao lado de ' tabelas ' para importar todas as tabelas e clique em ' Concluir '</span><span class="sxs-lookup"><span data-stu-id="363c3-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Etapa 3 do assistente](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="363c3-154">Depois que o processo de engenharia reversa for concluído, o novo modelo será adicionado ao seu projeto e aberto para que você exiba na Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="363c3-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="363c3-155">Um arquivo app. config também foi adicionado ao seu projeto com os detalhes de conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="363c3-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Inicial do modelo](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="363c3-157">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="363c3-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="363c3-158">Se você estiver trabalhando no Visual Studio 2010, há algumas etapas adicionais que você precisa seguir para atualizar para a versão mais recente do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="363c3-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="363c3-159">A atualização é importante porque fornece acesso a uma superfície de API aprimorada, que é muito mais fácil de usar, bem como as correções de bugs mais recentes.</span><span class="sxs-lookup"><span data-stu-id="363c3-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="363c3-160">Primeiro, precisamos obter a versão mais recente do Entity Framework do NuGet.</span><span class="sxs-lookup"><span data-stu-id="363c3-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="363c3-161">**Projeto – &gt; gerenciar pacotes NuGet...** 
    \*se você não tiver a opção **gerenciar pacotes NuGet...** , você deve instalar a [versão mais recente do NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) \*</span><span class="sxs-lookup"><span data-stu-id="363c3-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="363c3-162">Selecione a guia **online**</span><span class="sxs-lookup"><span data-stu-id="363c3-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="363c3-163">Selecionar o pacote do **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="363c3-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="363c3-164">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="363c3-164">Click **Install**</span></span>

<span data-ttu-id="363c3-165">Em seguida, precisamos trocar nosso modelo para gerar código que usa a API DbContext, que foi introduzida em versões posteriores do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="363c3-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="363c3-166">Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="363c3-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="363c3-167">Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**</span><span class="sxs-lookup"><span data-stu-id="363c3-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="363c3-168">Selecione o gerador de DbContext do EF **5. x para C @ no__t-1**, digite **BloggingModel** como o nome e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="363c3-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Modelo DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="363c3-170">4. Lendo & gravando dados</span><span class="sxs-lookup"><span data-stu-id="363c3-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="363c3-171">Agora que temos um modelo, é hora de usá-lo para acessar alguns dados.</span><span class="sxs-lookup"><span data-stu-id="363c3-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="363c3-172">As classes que usaremos para acessar os dados estão sendo geradas automaticamente para você com base no arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="363c3-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="363c3-173">*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010, os arquivos BloggingModel.tt e BloggingModel.Context.tt estarão diretamente sob seu projeto, e não aninhados no arquivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="363c3-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Classes geradas DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="363c3-175">Implemente o método Main em Program.cs, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="363c3-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="363c3-176">Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo blog.</span><span class="sxs-lookup"><span data-stu-id="363c3-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="363c3-177">Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados ordenado alfabeticamente por título.</span><span class="sxs-lookup"><span data-stu-id="363c3-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="363c3-178">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="363c3-178">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="363c3-179">5. Lidando com alterações de banco de dados</span><span class="sxs-lookup"><span data-stu-id="363c3-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="363c3-180">Agora é hora de fazer algumas alterações no nosso esquema de banco de dados, quando fazemos essas alterações, também precisamos atualizar nosso modelo para refletir essas alterações.</span><span class="sxs-lookup"><span data-stu-id="363c3-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="363c3-181">A primeira etapa é fazer algumas alterações no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="363c3-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="363c3-182">Vamos adicionar uma tabela Users ao esquema.</span><span class="sxs-lookup"><span data-stu-id="363c3-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="363c3-183">Clique com o botão direito do mouse no banco de dados **DatabaseFirst. blog** em Gerenciador de servidores e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="363c3-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="363c3-184">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="363c3-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="363c3-185">Agora que o esquema está atualizado, é hora de atualizar o modelo com essas alterações.</span><span class="sxs-lookup"><span data-stu-id="363c3-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="363c3-186">Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione ' atualizar modelo do banco de dados... '. isso iniciará o assistente de atualização</span><span class="sxs-lookup"><span data-stu-id="363c3-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="363c3-187">Na guia adicionar do assistente de atualização, marque a caixa ao lado de tabelas, isso indica que queremos adicionar novas tabelas a partir do esquema.</span><span class="sxs-lookup"><span data-stu-id="363c3-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="363c3-188">@no__t guia atualização de 0The mostra todas as tabelas existentes no modelo que serão verificadas quanto a alterações durante a atualização. As guias excluir mostram todas as tabelas que foram removidas do esquema e também serão removidas do modelo como parte da atualização. As informações nessas duas guias são detectadas automaticamente e são fornecidas apenas para fins informativos, você não pode alterar as configurações. \*</span><span class="sxs-lookup"><span data-stu-id="363c3-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Assistente de atualização](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="363c3-190">Clique em concluir no assistente de atualização</span><span class="sxs-lookup"><span data-stu-id="363c3-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="363c3-191">O modelo agora é atualizado para incluir uma nova entidade de usuário que é mapeada para a tabela usuários que adicionamos ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="363c3-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Modelo atualizado](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="363c3-193">Resumo</span><span class="sxs-lookup"><span data-stu-id="363c3-193">Summary</span></span>

<span data-ttu-id="363c3-194">Neste tutorial, examinamos Database First desenvolvimento, que nos permitia criar um modelo no designer do EF com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="363c3-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="363c3-195">Em seguida, usamos esse modelo para ler e gravar alguns dados do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="363c3-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="363c3-196">Por fim, atualizamos o modelo para refletir as alterações feitas no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="363c3-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
