---
title: DataBinding com WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416592"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="45ca2-102">Associação de dados com o WPF</span><span class="sxs-lookup"><span data-stu-id="45ca2-102">Databinding with WPF</span></span>
<span data-ttu-id="45ca2-103">Este guia passo a passo mostra como associar tipos POCO a controles WPF em um formulário de "mestre-detalhes".</span><span class="sxs-lookup"><span data-stu-id="45ca2-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="45ca2-104">O aplicativo usa as APIs de Entity Framework para preencher objetos com dados do banco de dado, controlar alterações e manter dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="45ca2-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="45ca2-105">O modelo define dois tipos que participam de uma relação um-para-muitos: **Category** (principal\\mestre) e **Product** (dependente\\detalhe).</span><span class="sxs-lookup"><span data-stu-id="45ca2-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="45ca2-106">Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo aos controles do WPF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="45ca2-107">A estrutura de associação de dados do WPF permite a navegação entre objetos relacionados: a seleção de linhas no modo de exibição mestre faz com que a exibição de detalhes seja atualizada com os dados filho correspondentes.</span><span class="sxs-lookup"><span data-stu-id="45ca2-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="45ca2-108">As capturas de tela e as listagens de código neste passo a passos são obtidas de Visual Studio 2013, mas você pode concluir este passo a passos com o Visual Studio 2012 ou o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="45ca2-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="45ca2-109">Usar a opção ' Object ' para criar fontes de dados do WPF</span><span class="sxs-lookup"><span data-stu-id="45ca2-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="45ca2-110">Com a versão anterior do Entity Framework, usamos para recomendar o uso da opção **Database** ao criar uma nova fonte de dados com base em um modelo criado com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="45ca2-111">Isso ocorreu porque o designer geraria um contexto derivado de ObjectContext e classes de entidade derivadas de EntityObject.</span><span class="sxs-lookup"><span data-stu-id="45ca2-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="45ca2-112">Usar a opção de banco de dados ajudará você a escrever o melhor código para interagir com essa superfície de API.</span><span class="sxs-lookup"><span data-stu-id="45ca2-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="45ca2-113">Os designers do EF para Visual Studio 2012 e Visual Studio 2013 geram um contexto que deriva de DbContext junto com classes de entidade POCO simples.</span><span class="sxs-lookup"><span data-stu-id="45ca2-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="45ca2-114">Com o Visual Studio 2010, recomendamos alternar para um modelo de geração de código que usa DbContext, conforme descrito mais adiante neste guia.</span><span class="sxs-lookup"><span data-stu-id="45ca2-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="45ca2-115">Ao usar a superfície da API DbContext, você deve usar a opção **Object** ao criar uma nova fonte de dados, conforme mostrado neste passo a passos.</span><span class="sxs-lookup"><span data-stu-id="45ca2-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="45ca2-116">Se necessário, você pode [reverter para a geração de código baseada em ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para modelos criados com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="45ca2-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="45ca2-117">Pre-Requisites</span></span>

<span data-ttu-id="45ca2-118">Você precisa ter Visual Studio 2013, o Visual Studio 2012 ou o Visual Studio 2010 instalado para concluir este passo a passos.</span><span class="sxs-lookup"><span data-stu-id="45ca2-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="45ca2-119">Se você estiver usando o Visual Studio 2010, também precisará instalar o NuGet.</span><span class="sxs-lookup"><span data-stu-id="45ca2-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="45ca2-120">Para obter mais informações, consulte [instalando o NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="45ca2-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="45ca2-121">Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="45ca2-121">Create the Application</span></span>

-   <span data-ttu-id="45ca2-122">Abra o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45ca2-122">Open Visual Studio</span></span>
-   <span data-ttu-id="45ca2-123">**Arquivo-&gt; novo&gt; projeto....**</span><span class="sxs-lookup"><span data-stu-id="45ca2-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="45ca2-124">Selecione do **Windows** no painel esquerdo e **WPFApplication** no painel direito</span><span class="sxs-lookup"><span data-stu-id="45ca2-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="45ca2-125">Insira **WPFwithEFSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="45ca2-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="45ca2-126">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="45ca2-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="45ca2-127">Instalar o pacote NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="45ca2-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="45ca2-128">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="45ca2-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="45ca2-129">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="45ca2-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="45ca2-130">Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="45ca2-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="45ca2-131">Clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="45ca2-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="45ca2-132">Além do assembly do EntityFramework, uma referência a System. ComponentModel. Annotations também é adicionada.</span><span class="sxs-lookup"><span data-stu-id="45ca2-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="45ca2-133">Se o projeto tiver uma referência a System. Data. Entity, ele será removido quando o pacote do EntityFramework for instalado.</span><span class="sxs-lookup"><span data-stu-id="45ca2-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="45ca2-134">O assembly System. Data. Entity não é mais usado para aplicativos Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="45ca2-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="45ca2-135">Definir um modelo</span><span class="sxs-lookup"><span data-stu-id="45ca2-135">Define a Model</span></span>

<span data-ttu-id="45ca2-136">Neste tutorial, você pode optar por implementar um modelo usando Code First ou o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="45ca2-137">Conclua uma das duas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="45ca2-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="45ca2-138">Opção 1: definir um modelo usando Code First</span><span class="sxs-lookup"><span data-stu-id="45ca2-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="45ca2-139">Esta seção mostra como criar um modelo e seu banco de dados associado usando Code First.</span><span class="sxs-lookup"><span data-stu-id="45ca2-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="45ca2-140">Pule para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF</span><span class="sxs-lookup"><span data-stu-id="45ca2-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="45ca2-141">Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).</span><span class="sxs-lookup"><span data-stu-id="45ca2-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="45ca2-142">Adicione uma nova classe ao **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="45ca2-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="45ca2-143">Clique com o botão direito do mouse no nome do projeto</span><span class="sxs-lookup"><span data-stu-id="45ca2-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="45ca2-144">Selecione **Adicionar**e **novo item**</span><span class="sxs-lookup"><span data-stu-id="45ca2-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="45ca2-145">Selecione **classe** e insira do **produto** para o nome da classe</span><span class="sxs-lookup"><span data-stu-id="45ca2-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="45ca2-146">Substitua o **produto** definição de classe pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="45ca2-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="45ca2-147">A propriedade **Products** na classe **Category** e a propriedade **Category** na classe **Product** são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="45ca2-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="45ca2-148">Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="45ca2-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="45ca2-149">Além de definir entidades, você precisa definir uma classe derivada de DbContext e expõe DbSet&lt;TEntity&gt; Propriedades.</span><span class="sxs-lookup"><span data-stu-id="45ca2-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="45ca2-150">As propriedades DbSet&lt;TEntity&gt; permitem que o contexto saiba quais tipos você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="45ca2-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="45ca2-151">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="45ca2-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="45ca2-152">Adicione uma nova classe **ProductContext** ao projeto com a seguinte definição:</span><span class="sxs-lookup"><span data-stu-id="45ca2-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="45ca2-153">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="45ca2-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="45ca2-154">Opção 2: definir um modelo usando Database First</span><span class="sxs-lookup"><span data-stu-id="45ca2-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="45ca2-155">Esta seção mostra como usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="45ca2-156">Se você concluiu a seção anterior (**opção 1: definir um modelo usando Code First)** , ignore esta seção e vá direto para a seção **carregamento lento** .</span><span class="sxs-lookup"><span data-stu-id="45ca2-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="45ca2-157">Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="45ca2-157">Create an Existing Database</span></span>

<span data-ttu-id="45ca2-158">Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.</span><span class="sxs-lookup"><span data-stu-id="45ca2-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="45ca2-159">O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:</span><span class="sxs-lookup"><span data-stu-id="45ca2-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="45ca2-160">Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="45ca2-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="45ca2-161">Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="45ca2-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="45ca2-162">Vamos continuar e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="45ca2-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="45ca2-163">**Exibir-&gt; Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="45ca2-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="45ca2-164">Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**</span><span class="sxs-lookup"><span data-stu-id="45ca2-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="45ca2-165">Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado</span><span class="sxs-lookup"><span data-stu-id="45ca2-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="45ca2-167">Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou e insira **produtos** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="45ca2-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Adicionar LocalDB de conexão](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar conexão Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="45ca2-170">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="45ca2-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="45ca2-172">O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="45ca2-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="45ca2-173">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="45ca2-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="45ca2-174">Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="45ca2-174">Reverse Engineer Model</span></span>

<span data-ttu-id="45ca2-175">Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="45ca2-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="45ca2-176">**Projeto-&gt; adicionar novo item...**</span><span class="sxs-lookup"><span data-stu-id="45ca2-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="45ca2-177">Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="45ca2-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="45ca2-178">Insira **ProductModel** como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="45ca2-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="45ca2-179">Isso iniciará o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="45ca2-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="45ca2-180">Selecione **gerar do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="45ca2-180">Select **Generate from Database** and click **Next**</span></span>

    ![Escolher conteúdo do modelo](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="45ca2-182">Selecione a conexão com o banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de conexão e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="45ca2-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Escolha sua conexão](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="45ca2-184">Clique na caixa de seleção ao lado de ' tabelas ' para importar todas as tabelas e clique em ' Concluir '</span><span class="sxs-lookup"><span data-stu-id="45ca2-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Escolha seus objetos](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="45ca2-186">Depois que o processo de engenharia reversa for concluído, o novo modelo será adicionado ao seu projeto e aberto para que você exiba na Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="45ca2-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="45ca2-187">Um arquivo app. config também foi adicionado ao seu projeto com os detalhes de conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="45ca2-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="45ca2-188">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="45ca2-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="45ca2-189">Se você estiver trabalhando no Visual Studio 2010, será necessário atualizar o designer do EF para usar a geração de código EF6.</span><span class="sxs-lookup"><span data-stu-id="45ca2-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="45ca2-190">Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="45ca2-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="45ca2-191">Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**</span><span class="sxs-lookup"><span data-stu-id="45ca2-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="45ca2-192">Selecione o **gerador de DbContext do EF 6. x para C\#,** insira **ProductsModel** como o nome e clique em Adicionar</span><span class="sxs-lookup"><span data-stu-id="45ca2-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="45ca2-193">Atualizando geração de código para associação de dados</span><span class="sxs-lookup"><span data-stu-id="45ca2-193">Updating code generation for data binding</span></span>

<span data-ttu-id="45ca2-194">O EF gera código do seu modelo usando modelos T4.</span><span class="sxs-lookup"><span data-stu-id="45ca2-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="45ca2-195">Os modelos fornecidos com o Visual Studio ou baixados da galeria do Visual Studio são destinados para uso geral.</span><span class="sxs-lookup"><span data-stu-id="45ca2-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="45ca2-196">Isso significa que as entidades geradas a partir desses modelos têm propriedades ICollection&lt;&gt; simples.</span><span class="sxs-lookup"><span data-stu-id="45ca2-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="45ca2-197">No entanto, ao fazer a vinculação de dados usando o WPF, é desejável usar **ObservableCollection** para propriedades de coleção para que o WPF possa controlar as alterações feitas nas coleções.</span><span class="sxs-lookup"><span data-stu-id="45ca2-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="45ca2-198">Para esse fim, iremos modificar os modelos para usar ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="45ca2-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="45ca2-199">Abra o **Gerenciador de soluções** e localize o arquivo **ProductModel. edmx**</span><span class="sxs-lookup"><span data-stu-id="45ca2-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="45ca2-200">Localize o arquivo **ProductModel.tt** que será aninhado no arquivo ProductModel. edmx</span><span class="sxs-lookup"><span data-stu-id="45ca2-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modelo de modelo de produto do WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="45ca2-202">Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="45ca2-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="45ca2-203">Localizar e substituir as duas ocorrências de "**ICollection**" por "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="45ca2-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="45ca2-204">Eles estão localizados aproximadamente nas linhas 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="45ca2-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="45ca2-205">Localizar e substituir a primeira ocorrência de "**HashSet**" por "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="45ca2-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="45ca2-206">Essa ocorrência está localizada aproximadamente na linha 50.</span><span class="sxs-lookup"><span data-stu-id="45ca2-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="45ca2-207">**Não** substitua a segunda ocorrência de HashSet encontrada posteriormente no código.</span><span class="sxs-lookup"><span data-stu-id="45ca2-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="45ca2-208">Localize e substitua a única ocorrência de "**System. Collections. Generic**" por "**System. Collections. ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="45ca2-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="45ca2-209">Isso está localizado aproximadamente na linha 424.</span><span class="sxs-lookup"><span data-stu-id="45ca2-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="45ca2-210">Salve o arquivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="45ca2-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="45ca2-211">Isso deve fazer com que o código das entidades seja regenerado.</span><span class="sxs-lookup"><span data-stu-id="45ca2-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="45ca2-212">Se o código não for regenerado automaticamente, clique com o botão direito do mouse em ProductModel.tt e escolha "executar ferramenta personalizada".</span><span class="sxs-lookup"><span data-stu-id="45ca2-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="45ca2-213">Se agora você abrir o arquivo Category.cs (que está aninhado em ProductModel.tt), verá que a coleção Products tem o tipo **observablecollection&lt;&gt;do produto** .</span><span class="sxs-lookup"><span data-stu-id="45ca2-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="45ca2-214">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="45ca2-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="45ca2-215">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="45ca2-215">Lazy Loading</span></span>

<span data-ttu-id="45ca2-216">A propriedade **Products** na classe **Category** e a propriedade **Category** na classe **Product** são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="45ca2-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="45ca2-217">Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="45ca2-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="45ca2-218">O EF oferece a você uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="45ca2-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="45ca2-219">Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que na primeira vez que você acessar cada propriedade de navegação, uma consulta separada será executada no banco de dados se o conteúdo ainda não estiver no contexto.</span><span class="sxs-lookup"><span data-stu-id="45ca2-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="45ca2-220">Ao usar tipos de entidade POCO, o EF alcança o carregamento lento criando instâncias de tipos de proxy derivados durante o tempo de execução e, em seguida, substituindo as propriedades virtuais em suas classes para adicionar o gancho de carregamento.</span><span class="sxs-lookup"><span data-stu-id="45ca2-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="45ca2-221">Para obter o carregamento lento de objetos relacionados, você deve declarar getters de propriedade de navegação como **público** e **virtual** (**substituível** em Visual Basic) e a classe não deve ser **selada** (não**substituível** em Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="45ca2-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="45ca2-222">Ao usar Database First Propriedades de navegação são automaticamente disponibilizadas para habilitar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="45ca2-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="45ca2-223">Na seção Code First, optamos por tornar as propriedades de navegação virtuais pela mesma razão</span><span class="sxs-lookup"><span data-stu-id="45ca2-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="45ca2-224">Associar objeto a controles</span><span class="sxs-lookup"><span data-stu-id="45ca2-224">Bind Object to Controls</span></span>

<span data-ttu-id="45ca2-225">Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo WPF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="45ca2-226">Clique duas vezes em **MainWindow. XAML** em Gerenciador de soluções para abrir o formulário principal</span><span class="sxs-lookup"><span data-stu-id="45ca2-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="45ca2-227">No menu principal, selecione **projeto-&gt; Adicionar nova fonte de dados...**</span><span class="sxs-lookup"><span data-stu-id="45ca2-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="45ca2-228">(no Visual Studio 2010, você precisa selecionar **dados-&gt; Adicionar nova fonte de dados...** )</span><span class="sxs-lookup"><span data-stu-id="45ca2-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="45ca2-229">Em escolher uma fonte de dados Typewindow, selecione **objeto** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="45ca2-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="45ca2-230">Na caixa de diálogo Selecionar os objetos de dados, desdobrar o **WPFwithEFSample** duas vezes e selecione **categoria**</span><span class="sxs-lookup"><span data-stu-id="45ca2-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="45ca2-231">*Não é necessário selecionar a fonte de dados do **produto** , pois vamos chegar a ela por meio da Propriedade do **produto**na fonte de dados **Category***</span><span class="sxs-lookup"><span data-stu-id="45ca2-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Selecionar objetos de dados](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="45ca2-233">Clique em **Concluir.**</span><span class="sxs-lookup"><span data-stu-id="45ca2-233">Click **Finish.**</span></span>
-   <span data-ttu-id="45ca2-234">A janela fontes de dados será aberta ao lado da janela MainWindow. XAML *se a janela fontes de dados não estiver aparecendo, selecione **exibir-&gt; outras fontes de dados de&gt; do Windows***</span><span class="sxs-lookup"><span data-stu-id="45ca2-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="45ca2-235">Pressione o ícone de pino para que a janela fontes de dados não seja ocultada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="45ca2-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="45ca2-236">Talvez seja necessário pressionar o botão atualizar se a janela já estava visível.</span><span class="sxs-lookup"><span data-stu-id="45ca2-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Fontes de dados](~/ef6/media/datasources.png)

-   <span data-ttu-id="45ca2-238">Selecione a fonte de dados **categoria** e arraste-a no formulário.</span><span class="sxs-lookup"><span data-stu-id="45ca2-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="45ca2-239">O seguinte ocorreu quando arrastamos esta fonte:</span><span class="sxs-lookup"><span data-stu-id="45ca2-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="45ca2-240">O recurso **categoryViewSource** e o controle **categoryDataGrid** foram adicionados ao XAML</span><span class="sxs-lookup"><span data-stu-id="45ca2-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="45ca2-241">A Propriedade DataContext no elemento pai Grid foi definida como "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="45ca2-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="45ca2-242"> O recurso **categoryViewSource** serve como uma fonte de associação para o elemento de grade pai de\\externa.</span><span class="sxs-lookup"><span data-stu-id="45ca2-242"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="45ca2-243">Os elementos da grade interna herdam o valor de DataContext da grade pai (a propriedade ItemsSource de categoryDataGrid é definida como "{Binding}")</span><span class="sxs-lookup"><span data-stu-id="45ca2-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="45ca2-244">Adicionando uma grade de detalhes</span><span class="sxs-lookup"><span data-stu-id="45ca2-244">Adding a Details Grid</span></span>

<span data-ttu-id="45ca2-245">Agora que temos uma grade para exibir categorias, vamos adicionar uma grade de detalhes para exibir os produtos associados.</span><span class="sxs-lookup"><span data-stu-id="45ca2-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="45ca2-246">Selecione a propriedade **Products** na fonte de dados **Category** e arraste-a no formulário.</span><span class="sxs-lookup"><span data-stu-id="45ca2-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="45ca2-247">O recurso **categoryProductsViewSource** e a grade **productDataGrid** são adicionados ao XAML</span><span class="sxs-lookup"><span data-stu-id="45ca2-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="45ca2-248">O caminho de associação deste recurso está definido como produtos</span><span class="sxs-lookup"><span data-stu-id="45ca2-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="45ca2-249">A estrutura de vinculação de dados do WPF garante que somente os produtos relacionados à categoria selecionada sejam exibidos no **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="45ca2-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="45ca2-250">Na caixa de ferramentas, arraste o **botão** para o formulário.</span><span class="sxs-lookup"><span data-stu-id="45ca2-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="45ca2-251">Defina a propriedade **Name** como **buttonSave** e a propriedade **Content** para **salvar**.</span><span class="sxs-lookup"><span data-stu-id="45ca2-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="45ca2-252">O formulário deve ser semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="45ca2-252">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="45ca2-254">Adicionar código que manipula a interação de dados</span><span class="sxs-lookup"><span data-stu-id="45ca2-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="45ca2-255">É hora de adicionar alguns manipuladores de eventos à janela principal.</span><span class="sxs-lookup"><span data-stu-id="45ca2-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="45ca2-256">Na janela XAML, clique no elemento de **janela&lt;** , isso seleciona a janela principal</span><span class="sxs-lookup"><span data-stu-id="45ca2-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="45ca2-257">Na janela **Propriedades** , escolha **eventos** no canto superior direito e clique duas vezes na caixa de texto à direita do rótulo **carregado**</span><span class="sxs-lookup"><span data-stu-id="45ca2-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Propriedades da janela principal](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="45ca2-259">Além disso, adicione o evento de **clique** para o botão **salvar** clicando duas vezes no botão salvar no designer.</span><span class="sxs-lookup"><span data-stu-id="45ca2-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="45ca2-260">Isso o levará ao code-behind para o formulário. agora, vamos editar o código para usar o ProductContext para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="45ca2-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="45ca2-261">Atualize o código para MainWindow, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="45ca2-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="45ca2-262">O código declara uma instância de execução longa do **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="45ca2-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="45ca2-263">O objeto **ProductContext** é usado para consultar e salvar dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="45ca2-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="45ca2-264">Em seguida, **Dispose ()** na instância **ProductContext** é chamado do método **OnClosing** substituído.</span><span class="sxs-lookup"><span data-stu-id="45ca2-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="45ca2-265"> Os comentários de código fornecem detalhes sobre o que o código faz.</span><span class="sxs-lookup"><span data-stu-id="45ca2-265"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="45ca2-266">Testar o aplicativo do WPF</span><span class="sxs-lookup"><span data-stu-id="45ca2-266">Test the WPF Application</span></span>

-   <span data-ttu-id="45ca2-267">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="45ca2-267">Compile and run the application.</span></span> <span data-ttu-id="45ca2-268">Se você usou Code First, verá que um banco de dados **WPFwithEFSample. ProductContext** é criado para você.</span><span class="sxs-lookup"><span data-stu-id="45ca2-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="45ca2-269">Insira um nome de categoria na grade superior e os nomes de produtos na grade inferior *não insiram nada em colunas de ID, porque a chave primária é gerada pelo banco de dados*</span><span class="sxs-lookup"><span data-stu-id="45ca2-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Janela principal com novas categorias e produtos](~/ef6/media/screen1.png)

-   <span data-ttu-id="45ca2-271">Pressione o botão **salvar** para salvar os dados no banco de dado</span><span class="sxs-lookup"><span data-stu-id="45ca2-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="45ca2-272">Após a chamada para **SaveChanges ()** do DbContext, as IDs são populadas com os valores gerados pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="45ca2-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="45ca2-273">Como chamamos a **atualização ()** após **SaveChanges ()** , os controles **DataGrid** também são atualizados com os novos valores.</span><span class="sxs-lookup"><span data-stu-id="45ca2-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Janela principal com IDs populadas](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="45ca2-275">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="45ca2-275">Additional Resources</span></span>

<span data-ttu-id="45ca2-276">Para saber mais sobre a ligação de dados com coleções usando o WPF, consulte [Este tópico](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) na documentação do WPF.</span><span class="sxs-lookup"><span data-stu-id="45ca2-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
