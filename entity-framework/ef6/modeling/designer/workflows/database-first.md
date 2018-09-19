---
title: Primeiro banco de dados - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284077"
---
# <a name="database-first"></a><span data-ttu-id="665ae-102">Primeiro banco de dados</span><span class="sxs-lookup"><span data-stu-id="665ae-102">Database First</span></span>
<span data-ttu-id="665ae-103">Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento de banco de dados primeiro usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="665ae-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="665ae-104">Banco de dados pela primeira vez permite que você faz engenharia reversa de um modelo de banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="665ae-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="665ae-105">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="665ae-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="665ae-106">As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="665ae-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="665ae-107">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="665ae-107">Watch the video</span></span>
<span data-ttu-id="665ae-108">Este vídeo fornece uma introdução ao desenvolvimento de banco de dados primeiro usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="665ae-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="665ae-109">Banco de dados pela primeira vez permite que você faz engenharia reversa de um modelo de banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="665ae-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="665ae-110">O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="665ae-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="665ae-111">As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="665ae-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="665ae-112">**Apresentado por**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="665ae-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="665ae-113">**Vídeo**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="665ae-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="665ae-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="665ae-114">Pre-Requisites</span></span>

<span data-ttu-id="665ae-115">Você precisará ter pelo menos o Visual Studio 2010 ou Visual Studio 2012 instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="665ae-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="665ae-116">Se você estiver usando o Visual Studio 2010, você também precisará ter [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="665ae-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="665ae-117">1. Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="665ae-117">1. Create an Existing Database</span></span>

<span data-ttu-id="665ae-118">Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.</span><span class="sxs-lookup"><span data-stu-id="665ae-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="665ae-119">O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="665ae-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="665ae-120">Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="665ae-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="665ae-121">Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="665ae-122">Vamos prosseguir e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="665ae-123">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="665ae-123">Open Visual Studio</span></span>
-   <span data-ttu-id="665ae-124">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="665ae-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="665ae-125">Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="665ae-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="665ae-126">Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados</span><span class="sxs-lookup"><span data-stu-id="665ae-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="665ae-128">Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado e insira **DatabaseFirst.Blogging** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="665ae-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Conexão do SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Conexão LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="665ae-131">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="665ae-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Criar caixa de diálogo banco de dados](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="665ae-133">O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="665ae-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="665ae-134">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="665ae-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="665ae-135">2. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="665ae-135">2. Create the Application</span></span>

<span data-ttu-id="665ae-136">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o primeiro banco de dados para executar o acesso a dados:</span><span class="sxs-lookup"><span data-stu-id="665ae-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="665ae-137">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="665ae-137">Open Visual Studio</span></span>
-   <span data-ttu-id="665ae-138">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="665ae-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="665ae-139">Selecione **Windows** no menu à esquerda e **aplicativo de Console**</span><span class="sxs-lookup"><span data-stu-id="665ae-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="665ae-140">Insira **DatabaseFirstSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="665ae-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="665ae-141">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="665ae-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="665ae-142">3. Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="665ae-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="665ae-143">Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="665ae-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="665ae-144">**Projeto -&gt; Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="665ae-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="665ae-145">Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="665ae-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="665ae-146">Insira **BloggingModel** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="665ae-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="665ae-147">Isso inicia o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="665ae-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="665ae-148">Selecione **gerar a partir do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="665ae-148">Select **Generate from Database** and click **Next**</span></span>

    ![Etapa 1 do Assistente](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="665ae-150">Selecione a conexão ao banco de dados que você criou na primeira seção, digite **BloggingContext** como o nome da cadeia de caracteres de conexão e clique **Avançar**</span><span class="sxs-lookup"><span data-stu-id="665ae-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Etapa 2 do Assistente](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="665ae-152">Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'</span><span class="sxs-lookup"><span data-stu-id="665ae-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Etapa 3 do Assistente](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="665ae-154">Depois de concluir o processo de engenharia reversa o novo modelo é adicionado ao seu projeto e aberto para visualização no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="665ae-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="665ae-155">Um arquivo App. config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Modelo inicial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="665ae-157">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="665ae-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="665ae-158">Se você estiver trabalhando no Visual Studio 2010, há algumas etapas adicionais que você precisa seguir para atualizar para a versão mais recente do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="665ae-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="665ae-159">A atualização é importante porque ele fornece acesso a uma superfície de API aprimorada, que é muito mais fácil de usar, bem como as correções de bug mais recentes.</span><span class="sxs-lookup"><span data-stu-id="665ae-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="665ae-160">Primeiro, precisamos obter a versão mais recente do Entity Framework do NuGet.</span><span class="sxs-lookup"><span data-stu-id="665ae-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="665ae-161">\*\*Projeto –&gt; gerenciar pacotes NuGet... \*\* 
     \*Se você não tiver o \**gerenciar pacotes NuGet... \*\* opção, você deve instalar o [versão mais recente do NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="665ae-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="665ae-162">Selecione o **Online** guia</span><span class="sxs-lookup"><span data-stu-id="665ae-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="665ae-163">Selecione o **EntityFramework** pacote</span><span class="sxs-lookup"><span data-stu-id="665ae-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="665ae-164">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="665ae-164">Click **Install**</span></span>

<span data-ttu-id="665ae-165">Em seguida, precisamos alternar nosso modelo para gerar o código que usa a API DbContext, que foi introduzida em versões posteriores do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="665ae-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="665ae-166">Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="665ae-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="665ae-167">Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**</span><span class="sxs-lookup"><span data-stu-id="665ae-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="665ae-168">Selecione o EF **5.x gerador DbContext para C\#**, insira **BloggingModel** como o nome e clique em **adicionar**</span><span class="sxs-lookup"><span data-stu-id="665ae-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Modelo de DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="665ae-170">4. Ler e gravar dados</span><span class="sxs-lookup"><span data-stu-id="665ae-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="665ae-171">Agora que temos um modelo que é hora de usá-lo para acessar alguns dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="665ae-172">As classes que vamos usar para acessar os dados são gerados automaticamente para você com base no arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="665ae-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="665ae-173">*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010 a BloggingModel.tt e BloggingModel.Context.tt arquivos diretamente em seu projeto, em vez de aninhado sob o arquivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="665ae-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Classes geradas DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="665ae-175">Implemente o método Main em Program.cs, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="665ae-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="665ae-176">Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir um novo Blog.</span><span class="sxs-lookup"><span data-stu-id="665ae-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="665ae-177">Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados classificado em ordem alfabética por título.</span><span class="sxs-lookup"><span data-stu-id="665ae-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="665ae-178">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="665ae-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="665ae-179">5. Lidando com alterações de banco de dados</span><span class="sxs-lookup"><span data-stu-id="665ae-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="665ae-180">Agora é hora de fazer algumas alterações em nosso esquema de banco de dados, quando fazemos essas alterações que também precisamos atualizar nosso modelo para refletir essas alterações.</span><span class="sxs-lookup"><span data-stu-id="665ae-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="665ae-181">A primeira etapa é fazer algumas alterações no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="665ae-182">Vamos adicionar uma tabela de usuários no esquema.</span><span class="sxs-lookup"><span data-stu-id="665ae-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="665ae-183">Clique com botão direito no **DatabaseFirst.Blogging** do banco de dados no Gerenciador de servidores e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="665ae-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="665ae-184">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="665ae-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="665ae-185">Agora que o esquema é atualizado, é hora de atualizar o modelo com essas alterações.</span><span class="sxs-lookup"><span data-stu-id="665ae-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="665ae-186">Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione 'modelo de atualização do banco de dados...', isso iniciará o Assistente de atualização</span><span class="sxs-lookup"><span data-stu-id="665ae-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="665ae-187">Na guia Add da verificação de Assistente de atualização, a caixa ao lado de tabelas, isso indica que desejamos adicionar quaisquer novas tabelas do esquema.</span><span class="sxs-lookup"><span data-stu-id="665ae-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="665ae-188">*A guia de atualização mostra todas as tabelas existentes no modelo que será verificado para que as alterações durante a atualização. As marcas de exclusão mostram todas as tabelas que foram removidas do esquema e também serão removidas do modelo como parte da atualização. As informações nessas duas guias são detectadas automaticamente e são fornecidas apenas para fins informativos, é possível alterar as configurações.*</span><span class="sxs-lookup"><span data-stu-id="665ae-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Assistente de atualização](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="665ae-190">Clique em Finish no Assistente de atualização</span><span class="sxs-lookup"><span data-stu-id="665ae-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="665ae-191">O modelo foi atualizado para incluir uma nova entidade de usuário que mapeia para a tabela de usuários que adicionamos ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Modelo atualizado](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="665ae-193">Resumo</span><span class="sxs-lookup"><span data-stu-id="665ae-193">Summary</span></span>

<span data-ttu-id="665ae-194">Neste passo a passo que vimos o desenvolvimento de banco de dados primeiro, que nos permitiu criar um modelo no Designer de EF com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="665ae-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="665ae-195">Em seguida, usamos esse modelo para ler e gravar alguns dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="665ae-196">Por fim, atualizamos o modelo para refletir as alterações feitas no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="665ae-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
