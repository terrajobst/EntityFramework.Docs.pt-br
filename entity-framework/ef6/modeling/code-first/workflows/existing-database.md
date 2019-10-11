---
title: Code First a um banco de dados existente-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182628"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="2ecf7-102">Code First a um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="2ecf7-102">Code First to an Existing Database</span></span>
<span data-ttu-id="2ecf7-103">Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Code First direcionamento a um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="2ecf7-104">Code First permite que você defina seu modelo usando classes C @ no__t-0 ou VB.Net.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="2ecf7-105">Opcionalmente, a configuração adicional pode ser executada usando atributos em suas classes e propriedades ou usando uma API fluente.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="2ecf7-106">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="2ecf7-106">Watch the video</span></span>
<span data-ttu-id="2ecf7-107">Este vídeo [agora está disponível no Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="2ecf7-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2ecf7-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2ecf7-108">Pre-Requisites</span></span>

<span data-ttu-id="2ecf7-109">Você precisará ter o **Visual Studio 2012** ou o **Visual Studio 2013** instalado para concluir este guia.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="2ecf7-110">Você também precisará da versão **6,1** (ou posterior) do **Entity Framework Tools para o Visual Studio** instalado.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="2ecf7-111">Consulte [obter Entity Framework](~/ef6/fundamentals/install.md) para obter informações sobre como instalar a versão mais recente do Entity Framework Tools.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="2ecf7-112">1. Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="2ecf7-112">1. Create an Existing Database</span></span>

<span data-ttu-id="2ecf7-113">Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="2ecf7-114">Vamos continuar e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="2ecf7-115">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ecf7-115">Open Visual Studio</span></span>
-   <span data-ttu-id="2ecf7-116">**Exibir-&gt; Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="2ecf7-117">Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="2ecf7-118">Se você ainda não se conectou a um banco de dados do **Gerenciador de servidores** antes de precisar selecionar **Microsoft SQL Server** como a fonte de dado</span><span class="sxs-lookup"><span data-stu-id="2ecf7-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="2ecf7-120">Conecte-se à sua instância do LocalDB e insira **Blogs** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="2ecf7-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Conexão LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="2ecf7-122">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Caixa de diálogo Criar banco de dados](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="2ecf7-124">O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="2ecf7-125">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="2ecf7-126">2. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2ecf7-126">2. Create the Application</span></span>

<span data-ttu-id="2ecf7-127">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa Code First para executar o acesso a dados:</span><span class="sxs-lookup"><span data-stu-id="2ecf7-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="2ecf7-128">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ecf7-128">Open Visual Studio</span></span>
-   <span data-ttu-id="2ecf7-129">**Arquivo-&gt; Projeto New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="2ecf7-130">Selecione **Windows** no menu à esquerda e no **aplicativo de console**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="2ecf7-131">Insira **CodeFirstExistingDatabaseSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="2ecf7-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="2ecf7-132">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="2ecf7-133">3. Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="2ecf7-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="2ecf7-134">Vamos usar o Entity Framework Tools para que o Visual Studio nos ajude a gerar um código inicial para mapear para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="2ecf7-135">Essas ferramentas estão apenas gerando código que você também pode digitar manualmente, se preferir.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="2ecf7-136">**Projeto-&gt; adicionar novo item...**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="2ecf7-137">Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="2ecf7-138">Insira **BloggingContext** como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="2ecf7-139">Isso iniciará o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="2ecf7-140">Selecione **Code First do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-140">Select **Code First from Database** and click **Next**</span></span>

    ![Assistente One CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="2ecf7-142">Selecione a conexão com o banco de dados que você criou na primeira seção e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Assistente dois CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="2ecf7-144">Clique na caixa de seleção ao lado de **tabelas** para importar todas as tabelas e clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="2ecf7-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Assistente de três CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="2ecf7-146">Depois que o processo de engenharia reversa for concluído, vários itens serão adicionados ao projeto, vamos dar uma olhada no que foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="2ecf7-147">arquivo de configuração</span><span class="sxs-lookup"><span data-stu-id="2ecf7-147">Configuration file</span></span>

<span data-ttu-id="2ecf7-148">Um arquivo app. config foi adicionado ao projeto, esse arquivo contém a cadeia de conexão para o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="2ecf7-149">*You'll Observe algumas outras configurações no arquivo de configuração também, essas são configurações padrão do EF que informam Code First onde criar bancos de dados. Como estamos mapeando para um banco de dados existente, essas configurações serão ignoradas em nosso aplicativo.*</span><span class="sxs-lookup"><span data-stu-id="2ecf7-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="2ecf7-150">Contexto derivado</span><span class="sxs-lookup"><span data-stu-id="2ecf7-150">Derived Context</span></span>

<span data-ttu-id="2ecf7-151">Uma classe **BloggingContext** foi adicionada ao projeto.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="2ecf7-152">O contexto representa uma sessão com o banco de dados, o que nos permite consultar e salvar os dados.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="2ecf7-153">O contexto expõe um **DbSet @ no__t-1TEntity @ no__t-2** para cada tipo em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="2ecf7-154">Você também observará que o construtor padrão chama um construtor base usando a sintaxe **Name =** .</span><span class="sxs-lookup"><span data-stu-id="2ecf7-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="2ecf7-155">Isso informa Code First que a cadeia de conexão a ser usada para esse contexto deve ser carregada a partir do arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="2ecf7-156">*You sempre deve usar a sintaxe **Name =** quando você estiver usando uma cadeia de conexão no arquivo de configuração. Isso garante que, se a cadeia de conexão não estiver presente, Entity Framework será lançada em vez de criar um novo banco de dados por convenção.*</span><span class="sxs-lookup"><span data-stu-id="2ecf7-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="2ecf7-157">Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="2ecf7-157">Model classes</span></span>

<span data-ttu-id="2ecf7-158">Por fim, um **blog** e uma classe **post** também foram adicionados ao projeto.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="2ecf7-159">Essas são as classes de domínio que compõem nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="2ecf7-160">Você verá as anotações de dados aplicadas às classes para especificar a configuração em que as convenções de Code First não se alinhariam com a estrutura do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="2ecf7-161">Por exemplo, você verá a anotação **StringLength** em **blog.Name** e **blog. URL** , pois eles têm um comprimento máximo de **200** no banco de dados (o padrão de Code First é usar o comprimento de máximo suportado pelo provedor de banco de dados- **nvarchar (max)** em SQL Server).</span><span class="sxs-lookup"><span data-stu-id="2ecf7-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="2ecf7-162">4. Lendo & gravando dados</span><span class="sxs-lookup"><span data-stu-id="2ecf7-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="2ecf7-163">Agora que temos um modelo, é hora de usá-lo para acessar alguns dados.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="2ecf7-164">Implemente o método **Main** em **Program.cs** , conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="2ecf7-165">Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo **blog**.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="2ecf7-166">Em seguida, ele usa uma consulta LINQ para recuperar todos os **Blogs** do banco de dados ordenado alfabeticamente por **título**.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="2ecf7-167">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-167">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="2ecf7-168">E se meu banco de dados mudar?</span><span class="sxs-lookup"><span data-stu-id="2ecf7-168">What if My Database Changes?</span></span>

<span data-ttu-id="2ecf7-169">O assistente de Code First para banco de dados foi criado para gerar um conjunto de classes de ponto de partida que você pode ajustar e modificar.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="2ecf7-170">Se o esquema do banco de dados for alterado, você poderá editar manualmente as classes ou executar outra engenharia reversa para substituir as classes.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="2ecf7-171">Usando Migrações do Code First para um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="2ecf7-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="2ecf7-172">Se você quiser usar Migrações do Code First com um banco de dados existente, consulte [migrações do Code First para um banco de dados existente](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="2ecf7-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="2ecf7-173">Resumo</span><span class="sxs-lookup"><span data-stu-id="2ecf7-173">Summary</span></span>

<span data-ttu-id="2ecf7-174">Neste tutorial, examinamos Code First desenvolvimento usando um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="2ecf7-175">Usamos o Entity Framework Tools para que o Visual Studio reverta a engenharia de um conjunto de classes mapeadas para o banco de dados e pudesse ser usado para armazenar e recuperar os dados.</span><span class="sxs-lookup"><span data-stu-id="2ecf7-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
