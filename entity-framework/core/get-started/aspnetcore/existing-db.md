---
title: "Introdução ao ASP.NET Core – Banco de dados existente – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="7ddc1-102">Introdução ao EF Core no ASP.NET Core com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="7ddc1-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="7ddc1-103">O [SDK do .NET Core](https://www.microsoft.com/net/download/core) não dá mais suporte a `project.json` ou ao Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="7ddc1-104">Todos que trabalham com desenvolvimento no .NET Core são incentivados a [migrar do project.json para csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) e [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7ddc1-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="7ddc1-105">Neste passo a passo, você compilará um aplicativo MVC do ASP.NET Core que executa acesso básico aos dados usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="7ddc1-106">Você usará engenharia reversa para criar um modelo do Entity Framework com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="7ddc1-107">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ddc1-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7ddc1-108">Prerequisites</span></span>

<span data-ttu-id="7ddc1-109">Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:</span><span class="sxs-lookup"><span data-stu-id="7ddc1-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="7ddc1-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:</span><span class="sxs-lookup"><span data-stu-id="7ddc1-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="7ddc1-111">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="7ddc1-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="7ddc1-112">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="7ddc1-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="7ddc1-113">[SDK do .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="7ddc1-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="7ddc1-114">Banco de dados de blog</span><span class="sxs-lookup"><span data-stu-id="7ddc1-114">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="7ddc1-115">Banco de dados de blog</span><span class="sxs-lookup"><span data-stu-id="7ddc1-115">Blogging database</span></span>

<span data-ttu-id="7ddc1-116">Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="7ddc1-117">Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="7ddc1-118">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7ddc1-118">Open Visual Studio</span></span>
* <span data-ttu-id="7ddc1-119">**Ferramentas > Conectar ao Banco de Dados...**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="7ddc1-120">Selecione **Microsoft SQL Server** e clique em **Continuar**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="7ddc1-121">Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="7ddc1-122">Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="7ddc1-123">O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="7ddc1-124">Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="7ddc1-125">Copie o script, listado abaixo, no editor de consultas</span><span class="sxs-lookup"><span data-stu-id="7ddc1-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="7ddc1-126">Clique com o botão direito do mouse no editor de consultas e selecione **Executar**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="7ddc1-127">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="7ddc1-127">Create a new project</span></span>

* <span data-ttu-id="7ddc1-128">Abra o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7ddc1-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="7ddc1-129">**Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="7ddc1-130">No menu à esquerda, selecione **Instalado > Modelos > Visual C# > Web**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="7ddc1-131">Selecione o modelo de projeto **Aplicativo Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="7ddc1-132">Insira **EFGetStarted.AspNetCore.ExistingDb** como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="7ddc1-133">Aguarde a caixa de diálogo **Novo Aplicativo Web ASP.NET Core** aparecer</span><span class="sxs-lookup"><span data-stu-id="7ddc1-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="7ddc1-134">Em **ASP.NET Core Templates 2.0** selecione o **Aplicativo Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="7ddc1-135">Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="7ddc1-136">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="7ddc1-137">Instalar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7ddc1-137">Install Entity Framework</span></span>

<span data-ttu-id="7ddc1-138">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7ddc1-139">Este passo a passo usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="7ddc1-140">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7ddc1-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7ddc1-141">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="7ddc1-142">Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="7ddc1-143">Usaremos Entity Framework Tools para criar um modelo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="7ddc1-144">Portanto, também instalaremos o pacote de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="7ddc1-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="7ddc1-145">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="7ddc1-146">Usaremos algumas ferramentas de scaffolding do ASP.NET Core para criar controladores e exibições posteriormente.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="7ddc1-147">Portanto, também instalaremos o pacote de design:</span><span class="sxs-lookup"><span data-stu-id="7ddc1-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="7ddc1-148">Execute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="7ddc1-149">Fazer engenharia reversa em seu modelo</span><span class="sxs-lookup"><span data-stu-id="7ddc1-149">Reverse engineer your model</span></span>

<span data-ttu-id="7ddc1-150">Agora é hora de criar o modelo EF com base em seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="7ddc1-151">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="7ddc1-152">Execute o seguinte comando para criar um modelo do banco de dados existente:</span><span class="sxs-lookup"><span data-stu-id="7ddc1-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="7ddc1-153">Se você receber um erro indicando `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="7ddc1-154">Especifique para quais tabelas você deseja gerar entidades adicionando o argumento `-Tables` para o comando acima.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="7ddc1-155">Por ex.:</span><span class="sxs-lookup"><span data-stu-id="7ddc1-155">E.g.</span></span> <span data-ttu-id="7ddc1-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="7ddc1-157">O processo de engenharia reversa criou classes de entidade (`Blog.cs` & `Post.cs`) e um contexto derivado (`BloggingContext.cs`) com base no esquema do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="7ddc1-158">As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="7ddc1-159">O contexto representa uma sessão com o banco de dados e permite que você veja e salve as instâncias das classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="7ddc1-160">Registre seu contexto com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="7ddc1-160">Register your context with dependency injection</span></span>

<span data-ttu-id="7ddc1-161">O conceito de injeção de dependência é central ao ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="7ddc1-162">Serviços (como `BloggingContext`) são registrados com injeção de dependência durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7ddc1-163">Agora, os componentes que exigem esses serviços (como os controladores de MVC) os recebem por meio de propriedades ou parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="7ddc1-164">Para saber mais sobre injeção de dependência, veja o artigo [Injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) no site do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="7ddc1-165">Remover configuração de contexto embutida</span><span class="sxs-lookup"><span data-stu-id="7ddc1-165">Remove inline context configuration</span></span>

<span data-ttu-id="7ddc1-166">No ASP.NET Core, a configuração geralmente é executada em **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="7ddc1-167">Para atender a esse padrão, mudaremos a configuração do provedor de banco de dados para **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="7ddc1-168">Abrir `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="7ddc1-169">Exclua o método `OnConfiguring(...)`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="7ddc1-170">Adicione o seguinte construtor, que permitirá que a configuração seja passada para o contexto pela injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="7ddc1-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="7ddc1-171">Registrar e configurar o contexto em Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7ddc1-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="7ddc1-172">Para que os controladores MVC usem o `BloggingContext`, vamos registrá-lo como um serviço.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="7ddc1-173">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="7ddc1-174">Adicione as declarações `using` a seguir ao início do método</span><span class="sxs-lookup"><span data-stu-id="7ddc1-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="7ddc1-175">Agora podemos usar o método `AddDbContext(...)` para registrá-lo como um serviço.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="7ddc1-176">Localize o método `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="7ddc1-177">Adicione o seguinte código para registrar o contexto como um serviço</span><span class="sxs-lookup"><span data-stu-id="7ddc1-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="7ddc1-178">Em um aplicativo real, normalmente você colocaria a cadeia de conexão em um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="7ddc1-179">Para simplificar, definimos isso no código.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="7ddc1-180">Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="7ddc1-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="7ddc1-181">Criar um controlador</span><span class="sxs-lookup"><span data-stu-id="7ddc1-181">Create a controller</span></span>

<span data-ttu-id="7ddc1-182">Em seguida, habilitaremos o scaffolding em nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="7ddc1-183">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador...**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="7ddc1-184">Selecione **Dependências Completas** e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="7ddc1-185">Você pode ignorar as instruções no arquivo `ScaffoldingReadMe.txt` aberto</span><span class="sxs-lookup"><span data-stu-id="7ddc1-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="7ddc1-186">Agora que o scaffolding está habilitado, é possível realizar o scaffolding de um controlador para a entidade `Blog`.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="7ddc1-187">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador...**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="7ddc1-188">Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Ok**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="7ddc1-189">Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="7ddc1-190">Clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="7ddc1-191">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="7ddc1-191">Run the application</span></span>

<span data-ttu-id="7ddc1-192">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="7ddc1-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="7ddc1-193">**Depurar > Iniciar Sem Depuração**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="7ddc1-194">O aplicativo será compilado e será aberto em um navegador da Web</span><span class="sxs-lookup"><span data-stu-id="7ddc1-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="7ddc1-195">Navegue até `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="7ddc1-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="7ddc1-196">Clique em **Criar Novo**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-196">Click **Create New**</span></span>
* <span data-ttu-id="7ddc1-197">Insira uma **Url** para o novo blog e clique em **Criar**</span><span class="sxs-lookup"><span data-stu-id="7ddc1-197">Enter a **Url** for the new blog and click **Create**</span></span>

![imagem](_static/create.png)

![imagem](_static/index-existing-db.png)
