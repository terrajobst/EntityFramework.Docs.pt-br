---
title: Code First para um banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 29f959265e0fd0d5e14c156519e6931fd8da0677
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995084"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="6698b-102">Code First para um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="6698b-102">Code First to an Existing Database</span></span>
<span data-ttu-id="6698b-103">Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Code First, direcionando um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6698b-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="6698b-104">Código primeiro permite que você defina seu modelo usando o C\# ou classes VB.Net.</span><span class="sxs-lookup"><span data-stu-id="6698b-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="6698b-105">Configuração adicional, opcionalmente, pode ser executada usando atributos em classes e propriedades ou usando uma API fluente.</span><span class="sxs-lookup"><span data-stu-id="6698b-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6698b-106">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="6698b-106">Watch the video</span></span>
<span data-ttu-id="6698b-107">Este vídeo [agora está disponível no Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="6698b-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6698b-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="6698b-108">Pre-Requisites</span></span>

<span data-ttu-id="6698b-109">Você precisará ter **Visual Studio 2012** ou **Visual Studio 2013** instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="6698b-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="6698b-110">Você também precisará versão **6.1** (ou posterior) da **Entity Framework Tools para Visual Studio** instalado.</span><span class="sxs-lookup"><span data-stu-id="6698b-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="6698b-111">Ver [obter o Entity Framework](~/ef6/fundamentals/install.md) para obter informações sobre como instalar a versão mais recente das ferramentas do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6698b-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="6698b-112">1. Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="6698b-112">1. Create an Existing Database</span></span>

<span data-ttu-id="6698b-113">Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.</span><span class="sxs-lookup"><span data-stu-id="6698b-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="6698b-114">Vamos prosseguir e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6698b-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="6698b-115">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6698b-115">Open Visual Studio</span></span>
-   <span data-ttu-id="6698b-116">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="6698b-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6698b-117">Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="6698b-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6698b-118">Se você ainda não tiver conectado a um banco de dados do **Gerenciador de servidores** antes de você precisará selecionar **Microsoft SQL Server** como fonte de dados</span><span class="sxs-lookup"><span data-stu-id="6698b-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="6698b-120">Conectar-se à instância do LocalDB e insira **blogs** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="6698b-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="6698b-122">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="6698b-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="6698b-124">O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="6698b-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="6698b-125">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="6698b-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="6698b-126">2. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6698b-126">2. Create the Application</span></span>

<span data-ttu-id="6698b-127">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Code First para executar o acesso a dados:</span><span class="sxs-lookup"><span data-stu-id="6698b-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="6698b-128">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6698b-128">Open Visual Studio</span></span>
-   <span data-ttu-id="6698b-129">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="6698b-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="6698b-130">Selecione **Windows** no menu à esquerda e **aplicativo de Console**</span><span class="sxs-lookup"><span data-stu-id="6698b-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="6698b-131">Insira **CodeFirstExistingDatabaseSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="6698b-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="6698b-132">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="6698b-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="6698b-133">3. Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="6698b-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="6698b-134">Vamos usar o Entity Framework Tools para Visual Studio para nos ajudar a gerar um código inicial para mapear para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6698b-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="6698b-135">Essas ferramentas são apenas gerar código que você também pode digitar manualmente se preferir.</span><span class="sxs-lookup"><span data-stu-id="6698b-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="6698b-136">**Projeto -&gt; Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="6698b-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="6698b-137">Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="6698b-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="6698b-138">Insira **BloggingContext** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="6698b-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="6698b-139">Isso inicia o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="6698b-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="6698b-140">Selecione **Code First do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="6698b-140">Select **Code First from Database** and click **Next**</span></span>

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="6698b-142">Selecione a conexão ao banco de dados que você criou na primeira seção e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="6698b-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="6698b-144">Clique na caixa de seleção ao lado **tabelas** para importar todas as tabelas e clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="6698b-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="6698b-146">Depois que o processo de engenharia reversa de um número de itens é concluído terão sido adicionadas ao projeto, vamos dar uma olhada no que foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="6698b-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="6698b-147">arquivo de configuração</span><span class="sxs-lookup"><span data-stu-id="6698b-147">Configuration file</span></span>

<span data-ttu-id="6698b-148">Um arquivo App. config foi adicionado ao projeto, esse arquivo contém a cadeia de caracteres de conexão ao banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6698b-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="6698b-149">*Você observará algumas outras configurações no arquivo de configuração muito, essas são as configurações padrão do EF que informam ao Code First onde criar bancos de dados. Uma vez que estamos estiver mapeando para um banco de dados existente, essas configurações serão ignoradas em nosso aplicativo.*</span><span class="sxs-lookup"><span data-stu-id="6698b-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="6698b-150">Contexto derivado</span><span class="sxs-lookup"><span data-stu-id="6698b-150">Derived Context</span></span>

<span data-ttu-id="6698b-151">Um **BloggingContext** classe foi adicionada ao projeto.</span><span class="sxs-lookup"><span data-stu-id="6698b-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="6698b-152">O contexto representa uma sessão com o banco de dados, que nos permite consultar e salvar dados.</span><span class="sxs-lookup"><span data-stu-id="6698b-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="6698b-153">Expõe o contexto de um **DbSet&lt;TEntity&gt;**  para cada tipo em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="6698b-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="6698b-154">Você também observará que o construtor padrão chama um construtor base usando o **nome =** sintaxe.</span><span class="sxs-lookup"><span data-stu-id="6698b-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="6698b-155">Isso informa ao Code First que a cadeia de caracteres de conexão a ser usado para este contexto deve ser carregada do arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="6698b-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="6698b-156">*Você sempre deve usar o **nome =** sintaxe quando você estiver usando uma cadeia de caracteres de conexão no arquivo de configuração. Isso garante que se a cadeia de caracteres de conexão não estiver presente, em seguida, Entity Framework gerará em vez de criar um novo banco de dados por convenção.*</span><span class="sxs-lookup"><span data-stu-id="6698b-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="6698b-157">Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="6698b-157">Model classes</span></span>

<span data-ttu-id="6698b-158">Por fim, uma **Blog** e **Post** classe também foram adicionados ao projeto.</span><span class="sxs-lookup"><span data-stu-id="6698b-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="6698b-159">Essas são as classes de domínio que compõem o nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="6698b-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="6698b-160">Você verá as anotações de dados aplicados às classes para especificar a configuração em que as convenções Code First não seriam alinha com a estrutura do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6698b-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="6698b-161">Por exemplo, você verá a **StringLength** anotação no **Blog.Name** e **Blog.Url** , pois eles têm um comprimento máximo de **200** no banco de dados (o padrão de Code First é usar o comprimento máximo com suporte pelo provedor de banco de dados - **nvarchar (max)** no SQL Server).</span><span class="sxs-lookup"><span data-stu-id="6698b-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="6698b-162">4. Ler e gravar dados</span><span class="sxs-lookup"><span data-stu-id="6698b-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="6698b-163">Agora que temos um modelo que é hora de usá-lo para acessar alguns dados.</span><span class="sxs-lookup"><span data-stu-id="6698b-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="6698b-164">Implemente a **principal** método na **Program.cs** conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="6698b-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="6698b-165">Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir uma nova **Blog**.</span><span class="sxs-lookup"><span data-stu-id="6698b-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="6698b-166">Em seguida, ele usa uma consulta LINQ para recuperar todos os **Blogs** do banco de dados classificado em ordem alfabética por **título**.</span><span class="sxs-lookup"><span data-stu-id="6698b-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="6698b-167">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="6698b-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="6698b-168">E se meu banco de dados é alterado?</span><span class="sxs-lookup"><span data-stu-id="6698b-168">What if My Database Changes?</span></span>

<span data-ttu-id="6698b-169">O Code First para o Assistente de banco de dados foi projetado para gerar um conjunto de ponto de partida de classes que você pode ajustar e modificar.</span><span class="sxs-lookup"><span data-stu-id="6698b-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="6698b-170">Se seu esquema de banco de dados for alterado você pode manualmente editar as classes ou executar outra engenharia reversa para substituir as classes.</span><span class="sxs-lookup"><span data-stu-id="6698b-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="6698b-171">Usando as migrações do Code First para um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="6698b-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="6698b-172">Se você quiser usar migrações do Code First com um banco de dados existente, consulte [migrações do Code First para um banco de dados](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="6698b-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="6698b-173">Resumo</span><span class="sxs-lookup"><span data-stu-id="6698b-173">Summary</span></span>

<span data-ttu-id="6698b-174">Neste passo a passo analisamos desenvolvimento Code First usando um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6698b-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="6698b-175">Usamos o Entity Framework Tools para Visual Studio para um conjunto de classes que são mapeados para o banco de dados e pode ser usado para armazenar e recuperar dados de engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="6698b-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
