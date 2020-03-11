---
title: DataBinding com WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419599"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="91e03-102">Associação de dados com WinForms</span><span class="sxs-lookup"><span data-stu-id="91e03-102">Databinding with WinForms</span></span>
<span data-ttu-id="91e03-103">Este guia passo a passo mostra como associar tipos POCO a controles Window Forms (WinForms) em um formulário "Master-Detail".</span><span class="sxs-lookup"><span data-stu-id="91e03-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="91e03-104">O aplicativo usa Entity Framework para preencher objetos com dados do banco de dados, controlar alterações e manter dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="91e03-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="91e03-105">O modelo define dois tipos que participam de uma relação um-para-muitos: Category (principal\\mestre) e Product (dependente\\detalhe).</span><span class="sxs-lookup"><span data-stu-id="91e03-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="91e03-106">Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo aos controles WinForms.</span><span class="sxs-lookup"><span data-stu-id="91e03-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="91e03-107">A estrutura de associação de dados do WinForms permite a navegação entre objetos relacionados: a seleção de linhas no modo de exibição mestre faz com que a exibição de detalhes seja atualizada com os dados filho correspondentes.</span><span class="sxs-lookup"><span data-stu-id="91e03-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="91e03-108">As capturas de tela e as listagens de código neste passo a passos são obtidas de Visual Studio 2013, mas você pode concluir este passo a passos com o Visual Studio 2012 ou o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="91e03-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="91e03-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="91e03-109">Pre-Requisites</span></span>

<span data-ttu-id="91e03-110">Você precisa ter Visual Studio 2013, o Visual Studio 2012 ou o Visual Studio 2010 instalado para concluir este passo a passos.</span><span class="sxs-lookup"><span data-stu-id="91e03-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="91e03-111">Se você estiver usando o Visual Studio 2010, também precisará instalar o NuGet.</span><span class="sxs-lookup"><span data-stu-id="91e03-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="91e03-112">Para obter mais informações, consulte [instalando o NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="91e03-112">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="91e03-113">Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="91e03-113">Create the Application</span></span>

-   <span data-ttu-id="91e03-114">Abra o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91e03-114">Open Visual Studio</span></span>
-   <span data-ttu-id="91e03-115">**Arquivo-&gt; novo&gt; projeto....**</span><span class="sxs-lookup"><span data-stu-id="91e03-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="91e03-116">Selecione **Windows** no painel esquerdo e **FormsApplication do Windows** no painel direito</span><span class="sxs-lookup"><span data-stu-id="91e03-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="91e03-117">Insira **WinFormswithEFSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="91e03-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="91e03-118">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="91e03-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="91e03-119">Instalar o pacote NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="91e03-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="91e03-120">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="91e03-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="91e03-121">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="91e03-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="91e03-122">Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="91e03-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="91e03-123">Clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="91e03-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="91e03-124">Além do assembly do EntityFramework, uma referência a System. ComponentModel. Annotations também é adicionada.</span><span class="sxs-lookup"><span data-stu-id="91e03-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="91e03-125">Se o projeto tiver uma referência a System. Data. Entity, ele será removido quando o pacote do EntityFramework for instalado.</span><span class="sxs-lookup"><span data-stu-id="91e03-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="91e03-126">O assembly System. Data. Entity não é mais usado para aplicativos Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="91e03-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="91e03-127">Implementando IListSource para coleções</span><span class="sxs-lookup"><span data-stu-id="91e03-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="91e03-128">As propriedades de coleção devem implementar a interface IListSource para habilitar a vinculação de dados bidirecional com classificação ao usar Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="91e03-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="91e03-129">Para fazer isso, Vamos estender ObservableCollection para adicionar a funcionalidade IListSource.</span><span class="sxs-lookup"><span data-stu-id="91e03-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="91e03-130">Adicione uma classe **ObservableListSource** ao projeto:</span><span class="sxs-lookup"><span data-stu-id="91e03-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="91e03-131">Clique com o botão direito do mouse no nome do projeto</span><span class="sxs-lookup"><span data-stu-id="91e03-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="91e03-132">Selecione **Adicionar-&gt; novo item**</span><span class="sxs-lookup"><span data-stu-id="91e03-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="91e03-133">Selecione **classe** e insira **ObservableListSource** para o nome da classe</span><span class="sxs-lookup"><span data-stu-id="91e03-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="91e03-134">Substitua o código gerado por padrão pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="91e03-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="91e03-135">*Essa classe habilita a vinculação de dados bidirecional, bem como a classificação. A classe deriva de ObservableCollection&lt;T&gt; e adiciona uma implementação explícita de IListSource. O método GetList () de IListSource é implementado para retornar uma implementação IBindingList que permanece em sincronia com a ObservableCollection. A implementação IBindingList gerada por tobindlist dá suporte à classificação. O método de extensão tobindlist é definido no assembly do EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="91e03-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="91e03-136">Definir um modelo</span><span class="sxs-lookup"><span data-stu-id="91e03-136">Define a Model</span></span>

<span data-ttu-id="91e03-137">Neste tutorial, você pode optar por implementar um modelo usando Code First ou o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="91e03-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="91e03-138">Conclua uma das duas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="91e03-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="91e03-139">Opção 1: definir um modelo usando Code First</span><span class="sxs-lookup"><span data-stu-id="91e03-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="91e03-140">Esta seção mostra como criar um modelo e seu banco de dados associado usando Code First.</span><span class="sxs-lookup"><span data-stu-id="91e03-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="91e03-141">Pule para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF</span><span class="sxs-lookup"><span data-stu-id="91e03-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="91e03-142">Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).</span><span class="sxs-lookup"><span data-stu-id="91e03-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="91e03-143">Adicionar uma nova classe de **produto** ao projeto</span><span class="sxs-lookup"><span data-stu-id="91e03-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="91e03-144">Substitua o código gerado por padrão pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="91e03-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="91e03-145">Adicione uma classe de **categoria** ao projeto.</span><span class="sxs-lookup"><span data-stu-id="91e03-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="91e03-146">Substitua o código gerado por padrão pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="91e03-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="91e03-147">Além de definir entidades, você precisa definir uma classe derivada de **DbContext** e expõe **DbSet&lt;TEntity&gt;** Propriedades.</span><span class="sxs-lookup"><span data-stu-id="91e03-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="91e03-148">As propriedades **DbSet** permitem que o contexto saiba quais tipos você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="91e03-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="91e03-149">Os tipos **DbContext** e **DbSet** são definidos no assembly do EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="91e03-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="91e03-150">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="91e03-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="91e03-151">Adicione uma nova classe **ProductContext** ao projeto.</span><span class="sxs-lookup"><span data-stu-id="91e03-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="91e03-152">Substitua o código gerado por padrão pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="91e03-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="91e03-153">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="91e03-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="91e03-154">Opção 2: definir um modelo usando Database First</span><span class="sxs-lookup"><span data-stu-id="91e03-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="91e03-155">Esta seção mostra como usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="91e03-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="91e03-156">Se você concluiu a seção anterior (**opção 1: definir um modelo usando Code First)** , ignore esta seção e vá direto para a seção **carregamento lento** .</span><span class="sxs-lookup"><span data-stu-id="91e03-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="91e03-157">Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="91e03-157">Create an Existing Database</span></span>

<span data-ttu-id="91e03-158">Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.</span><span class="sxs-lookup"><span data-stu-id="91e03-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="91e03-159">O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:</span><span class="sxs-lookup"><span data-stu-id="91e03-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="91e03-160">Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="91e03-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="91e03-161">Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="91e03-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="91e03-162">Vamos continuar e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91e03-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="91e03-163">**Exibir-&gt; Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="91e03-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="91e03-164">Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**</span><span class="sxs-lookup"><span data-stu-id="91e03-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="91e03-165">Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado</span><span class="sxs-lookup"><span data-stu-id="91e03-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="91e03-167">Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou e insira **produtos** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="91e03-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Adicionar LocalDB de conexão](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar conexão Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="91e03-170">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="91e03-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="91e03-172">O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="91e03-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="91e03-173">Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="91e03-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="91e03-174">Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="91e03-174">Reverse Engineer Model</span></span>

<span data-ttu-id="91e03-175">Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="91e03-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="91e03-176">**Projeto-&gt; adicionar novo item...**</span><span class="sxs-lookup"><span data-stu-id="91e03-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="91e03-177">Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="91e03-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="91e03-178">Insira **ProductModel** como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="91e03-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="91e03-179">Isso iniciará o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="91e03-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="91e03-180">Selecione **gerar do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="91e03-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="91e03-182">Selecione a conexão com o banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de conexão e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="91e03-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Escolha sua conexão](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="91e03-184">Clique na caixa de seleção ao lado de ' tabelas ' para importar todas as tabelas e clique em ' Concluir '</span><span class="sxs-lookup"><span data-stu-id="91e03-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Escolha seus objetos](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="91e03-186">Depois que o processo de engenharia reversa for concluído, o novo modelo será adicionado ao seu projeto e aberto para que você exiba na Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="91e03-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="91e03-187">Um arquivo app. config também foi adicionado ao seu projeto com os detalhes de conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91e03-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="91e03-188">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="91e03-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="91e03-189">Se você estiver trabalhando no Visual Studio 2010, será necessário atualizar o designer do EF para usar a geração de código EF6.</span><span class="sxs-lookup"><span data-stu-id="91e03-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="91e03-190">Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="91e03-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="91e03-191">Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**</span><span class="sxs-lookup"><span data-stu-id="91e03-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="91e03-192">Selecione o **gerador de DbContext do EF 6. x para C\#,** insira **ProductsModel** como o nome e clique em Adicionar</span><span class="sxs-lookup"><span data-stu-id="91e03-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="91e03-193">Atualizando geração de código para associação de dados</span><span class="sxs-lookup"><span data-stu-id="91e03-193">Updating code generation for data binding</span></span>

<span data-ttu-id="91e03-194">O EF gera código do seu modelo usando modelos T4.</span><span class="sxs-lookup"><span data-stu-id="91e03-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="91e03-195">Os modelos fornecidos com o Visual Studio ou baixados da galeria do Visual Studio são destinados para uso geral.</span><span class="sxs-lookup"><span data-stu-id="91e03-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="91e03-196">Isso significa que as entidades geradas a partir desses modelos têm propriedades ICollection&lt;&gt; simples.</span><span class="sxs-lookup"><span data-stu-id="91e03-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="91e03-197">No entanto, ao fazer a ligação de dados, é desejável ter propriedades de coleção que implementam IListSource.</span><span class="sxs-lookup"><span data-stu-id="91e03-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="91e03-198">É por isso que criamos a classe ObservableListSource acima e agora vamos modificar os modelos para usar essa classe.</span><span class="sxs-lookup"><span data-stu-id="91e03-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="91e03-199">Abra o **Gerenciador de soluções** e localize o arquivo **ProductModel. edmx**</span><span class="sxs-lookup"><span data-stu-id="91e03-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="91e03-200">Localize o arquivo **ProductModel.tt** que será aninhado no arquivo ProductModel. edmx</span><span class="sxs-lookup"><span data-stu-id="91e03-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modelo de modelo de produto](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="91e03-202">Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91e03-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="91e03-203">Localizar e substituir as duas ocorrências de "**ICollection**" por "**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="91e03-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="91e03-204">Elas estão localizadas em aproximadamente as linhas 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="91e03-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="91e03-205">Localize e substitua a primeira ocorrência de "**HashSet**" por "**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="91e03-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="91e03-206">Essa ocorrência está localizada em aproximadamente a linha 50.</span><span class="sxs-lookup"><span data-stu-id="91e03-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="91e03-207">**Não** substitua a segunda ocorrência de HashSet encontrada posteriormente no código.</span><span class="sxs-lookup"><span data-stu-id="91e03-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="91e03-208">Salve o arquivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="91e03-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="91e03-209">Isso deve fazer com que o código das entidades seja regenerado.</span><span class="sxs-lookup"><span data-stu-id="91e03-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="91e03-210">Se o código não for regenerado automaticamente, clique com o botão direito do mouse em ProductModel.tt e escolha "executar ferramenta personalizada".</span><span class="sxs-lookup"><span data-stu-id="91e03-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="91e03-211">Se agora você abrir o arquivo Category.cs (que está aninhado em ProductModel.tt), verá que a coleção Products tem o tipo **ObservableListSource&lt;Product&gt;** .</span><span class="sxs-lookup"><span data-stu-id="91e03-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="91e03-212">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="91e03-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="91e03-213">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="91e03-213">Lazy Loading</span></span>

<span data-ttu-id="91e03-214">A propriedade **Products** na classe **Category** e a propriedade **Category** na classe **Product** são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="91e03-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="91e03-215">Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="91e03-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="91e03-216">O EF oferece a você uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="91e03-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="91e03-217">Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que na primeira vez que você acessar cada propriedade de navegação, uma consulta separada será executada no banco de dados se o conteúdo ainda não estiver no contexto.</span><span class="sxs-lookup"><span data-stu-id="91e03-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="91e03-218">Ao usar tipos de entidade POCO, o EF alcança o carregamento lento criando instâncias de tipos de proxy derivados durante o tempo de execução e, em seguida, substituindo as propriedades virtuais em suas classes para adicionar o gancho de carregamento.</span><span class="sxs-lookup"><span data-stu-id="91e03-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="91e03-219">Para obter o carregamento lento de objetos relacionados, você deve declarar getters de propriedade de navegação como **público** e **virtual** (**substituível** em Visual Basic) e a classe não deve ser **selada** (não**substituível** em Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="91e03-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="91e03-220">Ao usar Database First Propriedades de navegação são automaticamente disponibilizadas para habilitar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="91e03-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="91e03-221">Na seção Code First, optamos por tornar as propriedades de navegação virtuais pela mesma razão</span><span class="sxs-lookup"><span data-stu-id="91e03-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="91e03-222">Associar objeto a controles</span><span class="sxs-lookup"><span data-stu-id="91e03-222">Bind Object to Controls</span></span>

<span data-ttu-id="91e03-223">Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo WinForms.</span><span class="sxs-lookup"><span data-stu-id="91e03-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="91e03-224">No menu principal, selecione **projeto-&gt; Adicionar nova fonte de dados...**</span><span class="sxs-lookup"><span data-stu-id="91e03-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="91e03-225">(no Visual Studio 2010, você precisa selecionar **dados-&gt; Adicionar nova fonte de dados...** )</span><span class="sxs-lookup"><span data-stu-id="91e03-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="91e03-226">Na janela escolher um tipo de fonte de dados, selecione **objeto** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="91e03-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="91e03-227">Na caixa de diálogo Selecionar os objetos de dados, desdobrar o **WinFormswithEFSample** duas vezes e selecione **categoria** não há necessidade de selecionar a fonte de dados do produto, pois vamos chegar a ela por meio da Propriedade do produto na fonte de dados Category.</span><span class="sxs-lookup"><span data-stu-id="91e03-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![fonte de dados](~/ef6/media/datasource.png)

-   <span data-ttu-id="91e03-229">Clique em **Concluir.**</span><span class="sxs-lookup"><span data-stu-id="91e03-229">Click **Finish.**</span></span>
    <span data-ttu-id="91e03-230">Se a janela fontes de dados não estiver aparecendo, selecione **Exibir-&gt; outras fontes de dados de&gt; do Windows**</span><span class="sxs-lookup"><span data-stu-id="91e03-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="91e03-231">Pressione o ícone de pino para que a janela fontes de dados não seja ocultada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="91e03-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="91e03-232">Talvez seja necessário pressionar o botão atualizar se a janela já estava visível.</span><span class="sxs-lookup"><span data-stu-id="91e03-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Fonte de dados 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="91e03-234">Em Gerenciador de Soluções, clique duas vezes no arquivo **Form1.cs** para abrir o formulário principal no designer.</span><span class="sxs-lookup"><span data-stu-id="91e03-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="91e03-235">Selecione a fonte de dados **categoria** e arraste-a no formulário.</span><span class="sxs-lookup"><span data-stu-id="91e03-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="91e03-236">Por padrão, um novo DataGridView (**categoryDataGridView**) e controles da barra de ferramentas de navegação são adicionados ao designer.</span><span class="sxs-lookup"><span data-stu-id="91e03-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="91e03-237">Esses controles são vinculados aos componentes BindingSource (**categoryBindingSource**) e**categoryBindingNavigator**(navegador de associação) que também são criados.</span><span class="sxs-lookup"><span data-stu-id="91e03-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="91e03-238">Edite as colunas no **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="91e03-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="91e03-239">Queremos definir a coluna **CategoryID** como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="91e03-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="91e03-240">O valor para a propriedade **CategoryID** é gerado pelo banco de dados depois que os mesmos são salvos.</span><span class="sxs-lookup"><span data-stu-id="91e03-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="91e03-241">Clique com o botão direito do mouse no controle DataGridView e selecione Editar colunas...</span><span class="sxs-lookup"><span data-stu-id="91e03-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="91e03-242">Selecione a coluna CategoryId e defina ReadOnly como true</span><span class="sxs-lookup"><span data-stu-id="91e03-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="91e03-243">Pressione OK</span><span class="sxs-lookup"><span data-stu-id="91e03-243">Press OK</span></span>
-   <span data-ttu-id="91e03-244">Selecione produtos na fonte de dados categoria e arraste-o no formulário.</span><span class="sxs-lookup"><span data-stu-id="91e03-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="91e03-245">O productDataGridView e o productBindingSource são adicionados ao formulário.</span><span class="sxs-lookup"><span data-stu-id="91e03-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="91e03-246">Edite as colunas no productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="91e03-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="91e03-247">Queremos ocultar as colunas CategoryId e Category e definir ProductId como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="91e03-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="91e03-248">O valor para a propriedade ProductId é gerado pelo banco de dados depois que os mesmos são salvos.</span><span class="sxs-lookup"><span data-stu-id="91e03-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="91e03-249">Clique com o botão direito do mouse no controle DataGridView e selecione **Editar colunas...** .</span><span class="sxs-lookup"><span data-stu-id="91e03-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="91e03-250">Selecione a coluna **ProductID** e defina **ReadOnly** como **true**.</span><span class="sxs-lookup"><span data-stu-id="91e03-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="91e03-251">Selecione a coluna **CategoryID** e pressione o botão **remover** .</span><span class="sxs-lookup"><span data-stu-id="91e03-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="91e03-252">Faça o mesmo com a coluna **categoria** .</span><span class="sxs-lookup"><span data-stu-id="91e03-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="91e03-253">Pressione **OK**.</span><span class="sxs-lookup"><span data-stu-id="91e03-253">Press **OK**.</span></span>

    <span data-ttu-id="91e03-254">Até agora, nós associomos nossos controles DataGridView a componentes BindingSource no designer.</span><span class="sxs-lookup"><span data-stu-id="91e03-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="91e03-255">Na próxima seção, adicionaremos o código ao code-behind para definir categoryBindingSource. DataSource como a coleção de entidades que são rastreadas atualmente por DbContext.</span><span class="sxs-lookup"><span data-stu-id="91e03-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="91e03-256">Quando arrastamos e descartamos produtos sob a categoria, os WinForms cuidam da configuração da propriedade productsBindingSource. DataSource para a propriedade categoryBindingSource e productsBindingSource. DataMember para Products.</span><span class="sxs-lookup"><span data-stu-id="91e03-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="91e03-257">Devido a essa associação, somente os produtos que pertencem à categoria atualmente selecionada serão exibidos no productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="91e03-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="91e03-258">Habilite o botão **salvar** na barra de ferramentas de navegação clicando com o botão direito do mouse e selecionando **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="91e03-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Designer de formulário 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="91e03-260">Adicione o manipulador de eventos ao botão salvar clicando duas vezes no botão.</span><span class="sxs-lookup"><span data-stu-id="91e03-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="91e03-261">Isso adicionará o manipulador de eventos e o levará ao code-behind para o formulário.</span><span class="sxs-lookup"><span data-stu-id="91e03-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="91e03-262">O código para o **categoryBindingNavigatorSaveItem\_** manipulador de eventos de clique será adicionado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="91e03-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="91e03-263">Adicionar o código que manipula a interação de dados</span><span class="sxs-lookup"><span data-stu-id="91e03-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="91e03-264">Agora, vamos adicionar o código para usar o ProductContext para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="91e03-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="91e03-265">Atualize o código para a janela de formulário principal, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="91e03-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="91e03-266">O código declara uma instância de execução longa do ProductContext.</span><span class="sxs-lookup"><span data-stu-id="91e03-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="91e03-267">O objeto ProductContext é usado para consultar e salvar dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="91e03-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="91e03-268">O método Dispose () na instância ProductContext é então chamado a partir do método OnClosing substituído.</span><span class="sxs-lookup"><span data-stu-id="91e03-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="91e03-269">Os comentários de código fornecem detalhes sobre o que o código faz.</span><span class="sxs-lookup"><span data-stu-id="91e03-269">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="91e03-270">Testar o aplicativo Windows Forms</span><span class="sxs-lookup"><span data-stu-id="91e03-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="91e03-271">Compile e execute o aplicativo e você pode testar a funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="91e03-271">Compile and run the application and you can test out the functionality.</span></span>

    ![Formulário 1 antes de salvar](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="91e03-273">Depois de salvar as chaves geradas pelo repositório, elas são mostradas na tela.</span><span class="sxs-lookup"><span data-stu-id="91e03-273">After saving the store generated keys are shown on the screen.</span></span>

    ![Formulário 1 após salvar](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="91e03-275">Se você usou Code First, verá também que um banco de dados **WinFormswithEFSample. ProductContext** é criado para você.</span><span class="sxs-lookup"><span data-stu-id="91e03-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Pesquisador de objetos do servidor](~/ef6/media/serverobjexplorer.png)
