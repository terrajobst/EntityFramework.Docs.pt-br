---
title: Associação de dados com o WPF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: e6df90db17d39d3aa91275800a6414fed40fb5db
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251148"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="13f16-102">Associação de dados com o WPF</span><span class="sxs-lookup"><span data-stu-id="13f16-102">Databinding with WPF</span></span>
<span data-ttu-id="13f16-103">Este passo a passo mostra como associar controles WPF em um formulário de "master-detail" tipos de POCO.</span><span class="sxs-lookup"><span data-stu-id="13f16-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="13f16-104">O aplicativo usa as APIs do Entity Framework para preencher os objetos com dados do banco de dados, controlar as alterações e manter os dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="13f16-105">O modelo define dois tipos que participam no relacionamento um-para-muitos: **categoria** (entidade de segurança\\mestre) e **produto** (dependentes\\detalhes).</span><span class="sxs-lookup"><span data-stu-id="13f16-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="13f16-106">Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo para os controles do WPF.</span><span class="sxs-lookup"><span data-stu-id="13f16-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="13f16-107">A estrutura de vinculação de dados do WPF permite a navegação entre objetos relacionados: selecionar linhas no modo de exibição mestre faz com que a exibição de detalhes atualizar com os dados filho correspondentes.</span><span class="sxs-lookup"><span data-stu-id="13f16-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="13f16-108">As listagens de código neste passo a passo e capturas de tela são obtidas do Visual Studio 2013, mas você pode concluir este passo a passo com o Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="13f16-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="13f16-109">Use a opção de 'Object' para a criação de fontes de dados do WPF</span><span class="sxs-lookup"><span data-stu-id="13f16-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="13f16-110">Com a versão anterior do Entity Framework usamos recomendável usar o **banco de dados** opção ao criar uma nova fonte de dados baseado em um modelo criado com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="13f16-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="13f16-111">Isso acontecia porque o designer geraria um contexto que é derivada de ObjectContext e classes de entidade derivam de EntityObject.</span><span class="sxs-lookup"><span data-stu-id="13f16-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="13f16-112">Usando a opção de banco de dados seria ajudá-lo a escrever o código de práticas para interagir com essa superfície de API.</span><span class="sxs-lookup"><span data-stu-id="13f16-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="13f16-113">Os Designers do EF para Visual Studio 2012 e Visual Studio 2013 gerar um contexto que é derivada de DbContext, junto com as classes de entidade POCO simples.</span><span class="sxs-lookup"><span data-stu-id="13f16-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="13f16-114">Com o Visual Studio 2010, é recomendável alternar para um modelo de geração de código que usa DbContext, conforme descrito posteriormente neste passo a passo.</span><span class="sxs-lookup"><span data-stu-id="13f16-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="13f16-115">Ao usar a superfície da API DbContext que você deve usar o **objeto** opção ao criar uma nova fonte de dados, conforme mostrado neste passo a passo.</span><span class="sxs-lookup"><span data-stu-id="13f16-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="13f16-116">Se necessário, você pode [reverter para a geração de código de ObjectContext com base em](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para modelos criados com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="13f16-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="13f16-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="13f16-117">Pre-Requisites</span></span>

<span data-ttu-id="13f16-118">Você precisa ter o Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="13f16-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="13f16-119">Se você estiver usando o Visual Studio 2010, você também precisará instalar o NuGet.</span><span class="sxs-lookup"><span data-stu-id="13f16-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="13f16-120">Para obter mais informações, consulte [instalando o NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="13f16-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="13f16-121">Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="13f16-121">Create the Application</span></span>

-   <span data-ttu-id="13f16-122">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13f16-122">Open Visual Studio</span></span>
-   <span data-ttu-id="13f16-123">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="13f16-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="13f16-124">Selecione **Windows** no painel esquerdo e **WPFApplication** no painel à direita</span><span class="sxs-lookup"><span data-stu-id="13f16-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="13f16-125">Insira **WPFwithEFSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="13f16-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="13f16-126">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="13f16-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="13f16-127">Instale o pacote NuGet do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="13f16-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="13f16-128">No Gerenciador de soluções, clique com botão direito no **WinFormswithEFSample** projeto</span><span class="sxs-lookup"><span data-stu-id="13f16-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="13f16-129">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="13f16-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="13f16-130">Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote</span><span class="sxs-lookup"><span data-stu-id="13f16-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="13f16-131">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="13f16-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="13f16-132">Uma referência ao DataAnnotations também é adicionada além do assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="13f16-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="13f16-133">Se o projeto tem uma referência a System, em seguida, ele será removido quando o pacote EntityFramework é instalado.</span><span class="sxs-lookup"><span data-stu-id="13f16-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="13f16-134">O assembly System não é mais usado para aplicativos do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="13f16-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="13f16-135">Definir um modelo</span><span class="sxs-lookup"><span data-stu-id="13f16-135">Define a Model</span></span>

<span data-ttu-id="13f16-136">Neste passo a passo que você pode optar por implementar um modelo usando o EF Designer ou o Code First.</span><span class="sxs-lookup"><span data-stu-id="13f16-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="13f16-137">Conclua uma das duas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="13f16-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="13f16-138">Opção 1: Definir um modelo usando o Code First</span><span class="sxs-lookup"><span data-stu-id="13f16-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="13f16-139">Esta seção mostra como criar um modelo e seu banco de dados associado usando o Code First.</span><span class="sxs-lookup"><span data-stu-id="13f16-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="13f16-140">Vá para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para reverter a engenharia de seu modelo de banco de dados usando o designer EF</span><span class="sxs-lookup"><span data-stu-id="13f16-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="13f16-141">Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio).</span><span class="sxs-lookup"><span data-stu-id="13f16-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="13f16-142">Adicione uma nova classe para o **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="13f16-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="13f16-143">Clique com botão direito no nome do projeto</span><span class="sxs-lookup"><span data-stu-id="13f16-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="13f16-144">Selecione **adicione**, em seguida, **Novo Item**</span><span class="sxs-lookup"><span data-stu-id="13f16-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="13f16-145">Selecione **classe** e insira **produto** para o nome de classe</span><span class="sxs-lookup"><span data-stu-id="13f16-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="13f16-146">Substitua os **produto** definição com o código a seguir da classe:</span><span class="sxs-lookup"><span data-stu-id="13f16-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="13f16-147">O **produtos** propriedade no **categoria** classe e **categoria** propriedade o **produto** classe são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="13f16-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="13f16-148">No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="13f16-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="13f16-149">Além de definir entidades, você precisa definir uma classe que deriva de DbContext e expõe um DbSet&lt;TEntity&gt; propriedades.</span><span class="sxs-lookup"><span data-stu-id="13f16-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="13f16-150">O DbSet&lt;TEntity&gt; propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="13f16-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="13f16-151">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="13f16-152">Adicione um novo **ProductContext** classe ao projeto com a seguinte definição:</span><span class="sxs-lookup"><span data-stu-id="13f16-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="13f16-153">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="13f16-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="13f16-154">Opção 2: Definir um modelo usando Database First</span><span class="sxs-lookup"><span data-stu-id="13f16-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="13f16-155">Esta seção mostra como usar o primeiro banco de dados para fazer engenharia reversa seu modelo de banco de dados usando o EF designer.</span><span class="sxs-lookup"><span data-stu-id="13f16-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="13f16-156">Se você concluiu a seção anterior (**opção 1: definir um modelo usando o Code First)**, ignore esta seção e ir direto para o **o carregamento lento** seção.</span><span class="sxs-lookup"><span data-stu-id="13f16-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="13f16-157">Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="13f16-157">Create an Existing Database</span></span>

<span data-ttu-id="13f16-158">Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.</span><span class="sxs-lookup"><span data-stu-id="13f16-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="13f16-159">O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="13f16-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="13f16-160">Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="13f16-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="13f16-161">Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="13f16-162">Vamos prosseguir e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="13f16-163">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="13f16-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="13f16-164">Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="13f16-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="13f16-165">Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados</span><span class="sxs-lookup"><span data-stu-id="13f16-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="13f16-167">Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado e insira **produtos** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="13f16-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Adicionar Conexão LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar Conexão Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="13f16-170">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="13f16-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="13f16-172">O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="13f16-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="13f16-173">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="13f16-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="13f16-174">Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="13f16-174">Reverse Engineer Model</span></span>

<span data-ttu-id="13f16-175">Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="13f16-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="13f16-176">**Projeto -&gt; Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="13f16-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="13f16-177">Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="13f16-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="13f16-178">Insira **ProductModel** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="13f16-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="13f16-179">Isso inicia o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="13f16-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="13f16-180">Selecione **gerar a partir do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="13f16-180">Select **Generate from Database** and click **Next**</span></span>

    ![Escolha o modelo de conteúdo](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="13f16-182">Selecione a conexão ao banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de caracteres de conexão e clique **Avançar**</span><span class="sxs-lookup"><span data-stu-id="13f16-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Escolha sua Conexão](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="13f16-184">Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'</span><span class="sxs-lookup"><span data-stu-id="13f16-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Escolher objetos do](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="13f16-186">Depois de concluir o processo de engenharia reversa o novo modelo é adicionado ao seu projeto e aberto para visualização no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="13f16-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="13f16-187">Um arquivo App. config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="13f16-188">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="13f16-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="13f16-189">Se você estiver trabalhando no Visual Studio 2010, em seguida, você precisará atualizar o EF designer para usar a geração de código do EF6.</span><span class="sxs-lookup"><span data-stu-id="13f16-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="13f16-190">Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="13f16-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="13f16-191">Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**</span><span class="sxs-lookup"><span data-stu-id="13f16-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="13f16-192">Selecione o **EF 6.x gerador DbContext para C\#,** insira **ProductsModel** como o nome e clique em Adicionar</span><span class="sxs-lookup"><span data-stu-id="13f16-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="13f16-193">Atualizando a geração de código para a associação de dados</span><span class="sxs-lookup"><span data-stu-id="13f16-193">Updating code generation for data binding</span></span>

<span data-ttu-id="13f16-194">O EF gera o código de seu modelo usando os modelos T4.</span><span class="sxs-lookup"><span data-stu-id="13f16-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="13f16-195">Os modelos fornecidos com o Visual Studio ou baixado na Galeria do Visual Studio são destinados a uso de finalidade geral.</span><span class="sxs-lookup"><span data-stu-id="13f16-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="13f16-196">Isso significa que as entidades geradas a partir desses modelos tem simple ICollection&lt;T&gt; propriedades.</span><span class="sxs-lookup"><span data-stu-id="13f16-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="13f16-197">No entanto, ao fazer a dados de associação usando o WPF é desejável usar **ObservableCollection** para propriedades de coleção para que o WPF pode manter controle das alterações feitas às coleções.</span><span class="sxs-lookup"><span data-stu-id="13f16-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="13f16-198">Para esse fim, vamos modificar os modelos para usar ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="13f16-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="13f16-199">Abra o **Gerenciador de soluções** e localize **ProductModel.edmx** arquivo</span><span class="sxs-lookup"><span data-stu-id="13f16-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="13f16-200">Localizar o **ProductModel.tt** arquivo que será aninhado sob o arquivo ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="13f16-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modelo de produto do WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="13f16-202">Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13f16-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="13f16-203">Localizar e substituir as duas ocorrências de "**ICollection**"com"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="13f16-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="13f16-204">Eles estão localizados aproximadamente em linhas 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="13f16-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="13f16-205">Localizar e substituir a primeira ocorrência de "**HashSet**"com"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="13f16-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="13f16-206">Essa ocorrência está localizada aproximadamente na linha 50.</span><span class="sxs-lookup"><span data-stu-id="13f16-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="13f16-207">**Não** substituir a segunda ocorrência da HashSet encontrada posteriormente no código.</span><span class="sxs-lookup"><span data-stu-id="13f16-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="13f16-208">Localizar e substituir a ocorrência única de "**Collections**"com"**ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="13f16-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="13f16-209">Isso está localizado aproximadamente na linha 424.</span><span class="sxs-lookup"><span data-stu-id="13f16-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="13f16-210">Salve o arquivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="13f16-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="13f16-211">Isso deve fazer com que o código para entidades sejam regenerados.</span><span class="sxs-lookup"><span data-stu-id="13f16-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="13f16-212">Se o código não regenerar automaticamente, em seguida, clique com botão direito em ProductModel.tt e escolha "Executar ferramenta personalizada".</span><span class="sxs-lookup"><span data-stu-id="13f16-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="13f16-213">Se você agora, abra o arquivo de Category.cs (que é aninhado sob ProductModel.tt) e em seguida, você deverá ver a coleção de produtos tem o tipo **ObservableCollection&lt;produto&gt;**.</span><span class="sxs-lookup"><span data-stu-id="13f16-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="13f16-214">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="13f16-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="13f16-215">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="13f16-215">Lazy Loading</span></span>

<span data-ttu-id="13f16-216">O **produtos** propriedade no **categoria** classe e **categoria** propriedade o **produto** classe são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="13f16-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="13f16-217">No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="13f16-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="13f16-218">O EF oferece uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="13f16-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="13f16-219">Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que a primeira vez que você acessar cada propriedade de navegação uma consulta separada será executada no banco de dados se o conteúdo não ainda estiverem no contexto.</span><span class="sxs-lookup"><span data-stu-id="13f16-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="13f16-220">Ao usar tipos de entidade POCO, o EF atinge o carregamento lento criando instâncias de tipos de proxy derivado durante o tempo de execução e, em seguida, substituindo propriedades virtuais nas suas classes para adicionar o gancho do carregamento.</span><span class="sxs-lookup"><span data-stu-id="13f16-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="13f16-221">Para obter o carregamento lento de objetos relacionados, você deve declarar navegação getters de propriedade como **pública** e **virtual** (**Overridable** no Visual Basic), e você classe não deve ser **lacrado** (**NotOverridable** no Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="13f16-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="13f16-222">Quando o banco de dados usando as propriedades de navegação primeiro são feitas automaticamente virtuais para habilitar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="13f16-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="13f16-223">Na seção Code First que escolhemos tornar as propriedades de navegação virtual pelo mesmo motivo</span><span class="sxs-lookup"><span data-stu-id="13f16-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="13f16-224">Associar o objeto a controles</span><span class="sxs-lookup"><span data-stu-id="13f16-224">Bind Object to Controls</span></span>

<span data-ttu-id="13f16-225">Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo do WPF.</span><span class="sxs-lookup"><span data-stu-id="13f16-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="13f16-226">Clique duas vezes em **MainWindow. XAML** no Gerenciador de soluções para abrir o formulário principal</span><span class="sxs-lookup"><span data-stu-id="13f16-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="13f16-227">No menu principal, selecione **projeto -&gt; adicionar nova fonte de dados...**</span><span class="sxs-lookup"><span data-stu-id="13f16-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="13f16-228">(no Visual Studio 2010, você precisa selecionar **dados -&gt; adicionar nova fonte de dados...** )</span><span class="sxs-lookup"><span data-stu-id="13f16-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="13f16-229">Na escolha um Typewindow de fonte de dados, selecione **objeto** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="13f16-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="13f16-230">Na caixa de diálogo de objetos de dados selecione, desdobrar o **WPFwithEFSample** duas vezes e selecione **categoria**</span><span class="sxs-lookup"><span data-stu-id="13f16-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="13f16-231">*Não é necessário selecionar o **produto** fonte de dados, porque obteremos a ele por meio o **produto**da propriedade no **categoria** fonte de dados*</span><span class="sxs-lookup"><span data-stu-id="13f16-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Selecionar objetos de dados](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="13f16-233">Clique em **concluir.**</span><span class="sxs-lookup"><span data-stu-id="13f16-233">Click **Finish.**</span></span>
-   <span data-ttu-id="13f16-234">A janela fontes de dados é aberta ao lado da janela de MainWindow. XAML *se a janela fontes de dados não está aparecendo, selecione **exibição -&gt; outras Windows -&gt; fontes de dados***</span><span class="sxs-lookup"><span data-stu-id="13f16-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="13f16-235">Pressione o ícone de pino, portanto, a janela fontes de dados não auto ocultar.</span><span class="sxs-lookup"><span data-stu-id="13f16-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="13f16-236">Você talvez precise clicar no botão de atualização se a janela estiver visível.</span><span class="sxs-lookup"><span data-stu-id="13f16-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Data Sources](~/ef6/media/datasources.png)

-   <span data-ttu-id="13f16-238">Selecione o \* \* categoria \* \* fonte de dados e arraste-o no formulário.</span><span class="sxs-lookup"><span data-stu-id="13f16-238">Select the \*\*Category \*\*data source and drag it on the form.</span></span>

<span data-ttu-id="13f16-239">O seguinte aconteceu quando foi arrastado essa fonte:</span><span class="sxs-lookup"><span data-stu-id="13f16-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="13f16-240">O **categoryViewSource** recursos e o \* \* categoryDataGrid \* \* o controle foram adicionados ao XAML.</span><span class="sxs-lookup"><span data-stu-id="13f16-240">The **categoryViewSource** resource and the\*\* categoryDataGrid\*\* control were added to XAML.</span></span> <span data-ttu-id="13f16-241">Para obter mais informações sobre DataViewSources, consulte http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="13f16-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="13f16-242">A propriedade DataContext no elemento de grade pai foi definida como "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="13f16-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="13f16-243">O **categoryViewSource** recurso serve como uma origem da associação para o exterior\\elemento de grade pai.</span><span class="sxs-lookup"><span data-stu-id="13f16-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="13f16-244">Os elementos internos de grade, em seguida, herdam o valor de DataContext do pai (a propriedade de ItemsSource do categoryDataGrid é definida como "{Binding}") de grade.</span><span class="sxs-lookup"><span data-stu-id="13f16-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="13f16-245">Adicionar uma grade de detalhes</span><span class="sxs-lookup"><span data-stu-id="13f16-245">Adding a Details Grid</span></span>

<span data-ttu-id="13f16-246">Agora que temos uma grade para exibir categorias vamos adicionar uma grade de detalhes para exibir os produtos associados.</span><span class="sxs-lookup"><span data-stu-id="13f16-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="13f16-247">Selecione o \* \* produtos \* \* propriedade no \* \* categoria \* \* fonte de dados e arraste-o no formulário.</span><span class="sxs-lookup"><span data-stu-id="13f16-247">Select the \*\*Products \*\*property from under the \*\*Category \*\*data source and drag it on the form.</span></span>
    -   <span data-ttu-id="13f16-248">O **categoryProductsViewSource** recursos e **productDataGrid** grade são adicionados ao XAML</span><span class="sxs-lookup"><span data-stu-id="13f16-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="13f16-249">O caminho de associação para este recurso é definido como produtos</span><span class="sxs-lookup"><span data-stu-id="13f16-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="13f16-250">Estrutura de vinculação de dados do WPF garante que somente os produtos relacionados à categoria selecionada aparecem no **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="13f16-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="13f16-251">Na caixa de ferramentas, arraste **botão** para o formulário.</span><span class="sxs-lookup"><span data-stu-id="13f16-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="13f16-252">Definir a **nome** propriedade a ser **buttonSave** e o **conteúdo** propriedade **salvar**.</span><span class="sxs-lookup"><span data-stu-id="13f16-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="13f16-253">O formulário deve ser semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="13f16-253">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="13f16-255">Adicione o código que trata a interação de dados</span><span class="sxs-lookup"><span data-stu-id="13f16-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="13f16-256">É hora de adicionar alguns manipuladores de eventos para a janela principal.</span><span class="sxs-lookup"><span data-stu-id="13f16-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="13f16-257">Na janela do XAML, clique no  **&lt;janela** elemento, isso seleciona a janela principal</span><span class="sxs-lookup"><span data-stu-id="13f16-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="13f16-258">No **propriedades** janela escolher **eventos** no canto superior direito, clique duas vezes para a direita da caixa de texto a **Loaded** rótulo</span><span class="sxs-lookup"><span data-stu-id="13f16-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Propriedades da janela principal](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="13f16-260">Também adicione a **clique em** evento para o **salvar** botão clicando duas vezes no botão Salvar no designer.</span><span class="sxs-lookup"><span data-stu-id="13f16-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="13f16-261">Isso leva você ao code-behind para o formulário, agora vamos editar o código para usar o ProductContext para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="13f16-262">Atualize o código para a MainWindow, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="13f16-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="13f16-263">O código declara uma instância de execução longa da **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="13f16-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="13f16-264">O **ProductContext** objeto é usado para consultar e salvar dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="13f16-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="13f16-265">O **Dispose**() na **ProductContext** instância é chamada de substituído **OnClosing** método.</span><span class="sxs-lookup"><span data-stu-id="13f16-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="13f16-266">Os comentários do código fornecem detalhes sobre o que o código faz.</span><span class="sxs-lookup"><span data-stu-id="13f16-266">The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="13f16-267">Testar o aplicativo do WPF</span><span class="sxs-lookup"><span data-stu-id="13f16-267">Test the WPF Application</span></span>

-   <span data-ttu-id="13f16-268">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="13f16-268">Compile and run the application.</span></span> <span data-ttu-id="13f16-269">Se você usou o Code First e, em seguida, você verá que um **WPFwithEFSample.ProductContext** banco de dados é criado para você.</span><span class="sxs-lookup"><span data-stu-id="13f16-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="13f16-270">Insira um nome de categoria nos nomes de produto e de grade principais na grade inferior *não inserir nada nas colunas de ID, como a chave primária é gerada pelo banco de dados*</span><span class="sxs-lookup"><span data-stu-id="13f16-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Janela principal com produtos e as novas categorias](~/ef6/media/screen1.png)

-   <span data-ttu-id="13f16-272">Pressione a **salvar** botão para salvar os dados no banco de dados</span><span class="sxs-lookup"><span data-stu-id="13f16-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="13f16-273">Após a chamada para do DbContext **SaveChanges**(), as IDs são preenchidas com os valores de banco de dados gerado.</span><span class="sxs-lookup"><span data-stu-id="13f16-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="13f16-274">Pois chamamos **Refresh**() após **SaveChanges**() a **DataGrid** controles são atualizados com os novos valores.</span><span class="sxs-lookup"><span data-stu-id="13f16-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Janela principal com IDs preenchidas](~/ef6/media/screen2.png)
