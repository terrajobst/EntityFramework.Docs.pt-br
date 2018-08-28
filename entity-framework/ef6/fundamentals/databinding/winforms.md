---
title: Associação de dados com WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 7ceb8e85fe3d8f5ab9a5e58ef9c84599585d8f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994523"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="33183-102">Associação de dados com WinForms</span><span class="sxs-lookup"><span data-stu-id="33183-102">Databinding with WinForms</span></span>
<span data-ttu-id="33183-103">Este passo a passo mostra como associar os tipos de POCO para controles de Windows Forms (WinForms) em um formulário de "master-detail".</span><span class="sxs-lookup"><span data-stu-id="33183-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="33183-104">O aplicativo usa o Entity Framework para preencher os objetos com dados do banco de dados, controlar as alterações e manter os dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33183-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="33183-105">O modelo define dois tipos que participam no relacionamento um-para-muitos: categoria (entidade de segurança\\mestre) e o produto (dependentes\\detalhes).</span><span class="sxs-lookup"><span data-stu-id="33183-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="33183-106">Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo para os controles do WinForms.</span><span class="sxs-lookup"><span data-stu-id="33183-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="33183-107">A estrutura de vinculação de dados WinForms permite a navegação entre objetos relacionados: selecionar linhas no modo de exibição mestre faz com que a exibição de detalhes atualizar com os dados filho correspondentes.</span><span class="sxs-lookup"><span data-stu-id="33183-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="33183-108">As listagens de código neste passo a passo e capturas de tela são obtidas do Visual Studio 2013, mas você pode concluir este passo a passo com o Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="33183-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="33183-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="33183-109">Pre-Requisites</span></span>

<span data-ttu-id="33183-110">Você precisa ter o Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="33183-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="33183-111">Se você estiver usando o Visual Studio 2010, você também precisará instalar o NuGet.</span><span class="sxs-lookup"><span data-stu-id="33183-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="33183-112">Para obter mais informações, consulte [instalando o NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="33183-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="33183-113">Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="33183-113">Create the Application</span></span>

-   <span data-ttu-id="33183-114">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33183-114">Open Visual Studio</span></span>
-   <span data-ttu-id="33183-115">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="33183-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="33183-116">Selecione **Windows** no painel esquerdo e **FormsApplication Windows** no painel à direita</span><span class="sxs-lookup"><span data-stu-id="33183-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="33183-117">Insira **WinFormswithEFSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="33183-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="33183-118">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="33183-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="33183-119">Instale o pacote NuGet do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="33183-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="33183-120">No Gerenciador de soluções, clique com botão direito no **WinFormswithEFSample** projeto</span><span class="sxs-lookup"><span data-stu-id="33183-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="33183-121">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="33183-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="33183-122">Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote</span><span class="sxs-lookup"><span data-stu-id="33183-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="33183-123">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="33183-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="33183-124">Uma referência ao DataAnnotations também é adicionada além do assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="33183-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="33183-125">Se o projeto tem uma referência a System, em seguida, ele será removido quando o pacote EntityFramework é instalado.</span><span class="sxs-lookup"><span data-stu-id="33183-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="33183-126">O assembly System não é mais usado para aplicativos do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="33183-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="33183-127">Implementação de IListSource para coleções</span><span class="sxs-lookup"><span data-stu-id="33183-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="33183-128">Propriedades de coleção devem implementar a interface IListSource para habilitar a associação de dados bidirecional com a classificação ao usar o Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="33183-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="33183-129">Para fazer isso, vamos estender ObservableCollection para adicionar a funcionalidade de IListSource.</span><span class="sxs-lookup"><span data-stu-id="33183-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="33183-130">Adicionar um **ObservableListSource** classe ao projeto:</span><span class="sxs-lookup"><span data-stu-id="33183-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="33183-131">Clique com botão direito no nome do projeto</span><span class="sxs-lookup"><span data-stu-id="33183-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="33183-132">Selecione **Add -&gt; Novo Item**</span><span class="sxs-lookup"><span data-stu-id="33183-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="33183-133">Selecione **classe** e insira **ObservableListSource** para o nome de classe</span><span class="sxs-lookup"><span data-stu-id="33183-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="33183-134">Substitua o código gerado por padrão com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="33183-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="33183-135">*Essa classe permite que a associação, bem como classificação de dados bidirecional. A classe derive de ObservableCollection&lt;T&gt; e adiciona uma implementação explícita de IListSource. O método GetList() de IListSource é implementado para retornar uma implementação de IBindingList permaneça em sincronia com a ObservableCollection. A implementação de IBindingList gerada pelo ToBindingList dá suporte à classificação. O método de extensão ToBindingList é definido no assembly EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="33183-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

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
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="33183-136">Definir um modelo</span><span class="sxs-lookup"><span data-stu-id="33183-136">Define a Model</span></span>

<span data-ttu-id="33183-137">Neste passo a passo que você pode optar por implementar um modelo usando o EF Designer ou o Code First.</span><span class="sxs-lookup"><span data-stu-id="33183-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="33183-138">Conclua uma das duas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="33183-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="33183-139">Opção 1: Definir um modelo usando o Code First</span><span class="sxs-lookup"><span data-stu-id="33183-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="33183-140">Esta seção mostra como criar um modelo e seu banco de dados associado usando o Code First.</span><span class="sxs-lookup"><span data-stu-id="33183-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="33183-141">Vá para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para reverter a engenharia de seu modelo de banco de dados usando o designer EF</span><span class="sxs-lookup"><span data-stu-id="33183-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="33183-142">Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio).</span><span class="sxs-lookup"><span data-stu-id="33183-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="33183-143">Adicione um novo **produto** classe ao projeto</span><span class="sxs-lookup"><span data-stu-id="33183-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="33183-144">Substitua o código gerado por padrão com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="33183-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="33183-145">Adicionar um **categoria** classe ao projeto.</span><span class="sxs-lookup"><span data-stu-id="33183-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="33183-146">Substitua o código gerado por padrão com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="33183-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="33183-147">Além de definir entidades, você precisa definir uma classe que deriva de **DbContext** e expõe **DbSet&lt;TEntity&gt;**  propriedades.</span><span class="sxs-lookup"><span data-stu-id="33183-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="33183-148">O **DbSet** propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="33183-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="33183-149">O **DbContext** e **DbSet** tipos são definidos no assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="33183-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="33183-150">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33183-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="33183-151">Adicione um novo **ProductContext** classe ao projeto.</span><span class="sxs-lookup"><span data-stu-id="33183-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="33183-152">Substitua o código gerado por padrão com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="33183-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="33183-153">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="33183-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="33183-154">Opção 2: Definir um modelo usando Database First</span><span class="sxs-lookup"><span data-stu-id="33183-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="33183-155">Esta seção mostra como usar o primeiro banco de dados para fazer engenharia reversa seu modelo de banco de dados usando o EF designer.</span><span class="sxs-lookup"><span data-stu-id="33183-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="33183-156">Se você concluiu a seção anterior (**opção 1: definir um modelo usando o Code First)**, ignore esta seção e ir direto para o **o carregamento lento** seção.</span><span class="sxs-lookup"><span data-stu-id="33183-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="33183-157">Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="33183-157">Create an Existing Database</span></span>

<span data-ttu-id="33183-158">Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.</span><span class="sxs-lookup"><span data-stu-id="33183-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="33183-159">O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="33183-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="33183-160">Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.</span><span class="sxs-lookup"><span data-stu-id="33183-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="33183-161">Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33183-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="33183-162">Vamos prosseguir e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33183-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="33183-163">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="33183-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="33183-164">Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="33183-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="33183-165">Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados</span><span class="sxs-lookup"><span data-stu-id="33183-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="33183-167">Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado e insira **produtos** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="33183-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="33183-170">Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="33183-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="33183-172">O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**</span><span class="sxs-lookup"><span data-stu-id="33183-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="33183-173">Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="33183-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="33183-174">Modelo de engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="33183-174">Reverse Engineer Model</span></span>

<span data-ttu-id="33183-175">Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="33183-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="33183-176">**Projeto -&gt; Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="33183-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="33183-177">Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="33183-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="33183-178">Insira **ProductModel** como o nome e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="33183-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="33183-179">Isso inicia o **Assistente de modelo de dados de entidade**</span><span class="sxs-lookup"><span data-stu-id="33183-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="33183-180">Selecione **gerar a partir do banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="33183-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="33183-182">Selecione a conexão ao banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de caracteres de conexão e clique **Avançar**</span><span class="sxs-lookup"><span data-stu-id="33183-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="33183-184">Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'</span><span class="sxs-lookup"><span data-stu-id="33183-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="33183-186">Depois de concluir o processo de engenharia reversa o novo modelo é adicionado ao seu projeto e aberto para visualização no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="33183-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="33183-187">Um arquivo App. config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33183-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="33183-188">Etapas adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="33183-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="33183-189">Se você estiver trabalhando no Visual Studio 2010, em seguida, você precisará atualizar o EF designer para usar a geração de código do EF6.</span><span class="sxs-lookup"><span data-stu-id="33183-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="33183-190">Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="33183-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="33183-191">Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**</span><span class="sxs-lookup"><span data-stu-id="33183-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="33183-192">Selecione o **EF 6.x gerador DbContext para C\#,** insira **ProductsModel** como o nome e clique em Adicionar</span><span class="sxs-lookup"><span data-stu-id="33183-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="33183-193">Atualizando a geração de código para a associação de dados</span><span class="sxs-lookup"><span data-stu-id="33183-193">Updating code generation for data binding</span></span>

<span data-ttu-id="33183-194">O EF gera o código de seu modelo usando os modelos T4.</span><span class="sxs-lookup"><span data-stu-id="33183-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="33183-195">Os modelos fornecidos com o Visual Studio ou baixado na Galeria do Visual Studio são destinados a uso de finalidade geral.</span><span class="sxs-lookup"><span data-stu-id="33183-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="33183-196">Isso significa que as entidades geradas a partir desses modelos tem simple ICollection&lt;T&gt; propriedades.</span><span class="sxs-lookup"><span data-stu-id="33183-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="33183-197">No entanto, ao fazer a ligação de dados é interessante ter propriedades de coleção que implementam IListSource.</span><span class="sxs-lookup"><span data-stu-id="33183-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="33183-198">Isso é por isso que criamos a classe ObservableListSource acima e agora vamos modificar os modelos para tornar usar essa classe.</span><span class="sxs-lookup"><span data-stu-id="33183-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="33183-199">Abra o **Gerenciador de soluções** e localize **ProductModel.edmx** arquivo</span><span class="sxs-lookup"><span data-stu-id="33183-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="33183-200">Localizar o **ProductModel.tt** arquivo que será aninhado sob o arquivo ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="33183-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="33183-202">Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33183-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="33183-203">Localizar e substituir as duas ocorrências de "**ICollection**"com"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="33183-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="33183-204">Eles estão localizados em aproximadamente linhas 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="33183-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="33183-205">Localizar e substituir a primeira ocorrência de "**HashSet**"com"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="33183-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="33183-206">Essa ocorrência está localizada em linha aproximadamente 50.</span><span class="sxs-lookup"><span data-stu-id="33183-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="33183-207">**Não** substituir a segunda ocorrência da HashSet encontrada posteriormente no código.</span><span class="sxs-lookup"><span data-stu-id="33183-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="33183-208">Salve o arquivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="33183-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="33183-209">Isso deve fazer com que o código para entidades sejam regenerados.</span><span class="sxs-lookup"><span data-stu-id="33183-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="33183-210">Se o código não regenerar automaticamente, em seguida, clique com botão direito em ProductModel.tt e escolha "Executar ferramenta personalizada".</span><span class="sxs-lookup"><span data-stu-id="33183-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="33183-211">Se você agora, abra o arquivo de Category.cs (que é aninhado sob ProductModel.tt) e em seguida, você deverá ver a coleção de produtos tem o tipo **ObservableListSource&lt;produto&gt;**.</span><span class="sxs-lookup"><span data-stu-id="33183-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="33183-212">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="33183-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="33183-213">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="33183-213">Lazy Loading</span></span>

<span data-ttu-id="33183-214">O **produtos** propriedade no **categoria** classe e **categoria** propriedade o **produto** classe são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="33183-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="33183-215">No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="33183-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="33183-216">O EF oferece uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="33183-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="33183-217">Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que a primeira vez que você acessar cada propriedade de navegação uma consulta separada será executada no banco de dados se o conteúdo não ainda estiverem no contexto.</span><span class="sxs-lookup"><span data-stu-id="33183-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="33183-218">Ao usar tipos de entidade POCO, o EF atinge o carregamento lento criando instâncias de tipos de proxy derivado durante o tempo de execução e, em seguida, substituindo propriedades virtuais nas suas classes para adicionar o gancho do carregamento.</span><span class="sxs-lookup"><span data-stu-id="33183-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="33183-219">Para obter o carregamento lento de objetos relacionados, você deve declarar navegação getters de propriedade como **pública** e **virtual** (**Overridable** no Visual Basic), e você classe não deve ser **lacrado** (**NotOverridable** no Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="33183-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="33183-220">Quando o banco de dados usando as propriedades de navegação primeiro são feitas automaticamente virtuais para habilitar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="33183-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="33183-221">Na seção Code First que escolhemos tornar as propriedades de navegação virtual pelo mesmo motivo</span><span class="sxs-lookup"><span data-stu-id="33183-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="33183-222">Associar o objeto a controles</span><span class="sxs-lookup"><span data-stu-id="33183-222">Bind Object to Controls</span></span>

<span data-ttu-id="33183-223">Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo de WinForms.</span><span class="sxs-lookup"><span data-stu-id="33183-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="33183-224">No menu principal, selecione **projeto -&gt; adicionar nova fonte de dados...**</span><span class="sxs-lookup"><span data-stu-id="33183-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="33183-225">(no Visual Studio 2010, você precisa selecionar **dados -&gt; adicionar nova fonte de dados...** )</span><span class="sxs-lookup"><span data-stu-id="33183-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="33183-226">Na escolha de uma janela do tipo de fonte de dados, selecione **objeto** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="33183-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="33183-227">Na caixa de diálogo de objetos de dados selecione, desdobrar o **WinFormswithEFSample** duas vezes e selecione **categoria** não é necessário selecionar a fonte de dados do produto, porque obteremos a ele por meio do produto propriedade na fonte de dados de categoria.</span><span class="sxs-lookup"><span data-stu-id="33183-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![DataSource](~/ef6/media/datasource.png)

-   <span data-ttu-id="33183-229">Clique em **concluir.** 
     *Se a janela fontes de dados não está aparecendo, selecione * * * Exibir -&gt; outras Windows -&gt; fontes de dados**</span><span class="sxs-lookup"><span data-stu-id="33183-229">Click **Finish.**
*If the Data Sources window is not showing up, select***View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="33183-230">Pressione o ícone de pino, portanto, a janela fontes de dados não auto ocultar.</span><span class="sxs-lookup"><span data-stu-id="33183-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="33183-231">Você talvez precise clicar no botão de atualização se a janela estiver visível.</span><span class="sxs-lookup"><span data-stu-id="33183-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSource2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="33183-233">No Gerenciador de soluções, clique duas vezes o **Form1.cs** arquivo para abrir o formulário principal no designer.</span><span class="sxs-lookup"><span data-stu-id="33183-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="33183-234">Selecione o **categoria** fonte de dados e arraste-o no formulário.</span><span class="sxs-lookup"><span data-stu-id="33183-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="33183-235">Por padrão, um novo DataGridView (**categoryDataGridView**) e controles de barra de ferramentas de navegação são adicionados ao designer.</span><span class="sxs-lookup"><span data-stu-id="33183-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="33183-236">Esses controles estão vinculados ao BindingSource (**categoryBindingSource**) e associação Navigator (**categoryBindingNavigator**) componentes também são criados.</span><span class="sxs-lookup"><span data-stu-id="33183-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="33183-237">Editar as colunas sobre o **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="33183-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="33183-238">Queremos definir a **CategoryId** coluna como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="33183-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="33183-239">O valor para o **CategoryId** propriedade é gerada pelo banco de dados após salvar os dados.</span><span class="sxs-lookup"><span data-stu-id="33183-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="33183-240">O controle DataGridView com o botão direito e selecione Edit Columns...</span><span class="sxs-lookup"><span data-stu-id="33183-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="33183-241">Selecione a coluna CategoryId e definir ReadOnly para True</span><span class="sxs-lookup"><span data-stu-id="33183-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="33183-242">Pressione Okey</span><span class="sxs-lookup"><span data-stu-id="33183-242">Press OK</span></span>
-   <span data-ttu-id="33183-243">Selecione os produtos de fonte de dados de categoria e arraste-o no formulário.</span><span class="sxs-lookup"><span data-stu-id="33183-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="33183-244">O productDataGridView e productBindingSource são adicionados ao formulário.</span><span class="sxs-lookup"><span data-stu-id="33183-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="33183-245">Edite as colunas no productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="33183-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="33183-246">Queremos ocultar as colunas CategoryId e categoria e defina o ProductId como somente leitura.</span><span class="sxs-lookup"><span data-stu-id="33183-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="33183-247">O valor da propriedade ProductId é gerado pelo banco de dados após salvar os dados.</span><span class="sxs-lookup"><span data-stu-id="33183-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="33183-248">O controle DataGridView com o botão direito e selecione **Editar colunas...** .</span><span class="sxs-lookup"><span data-stu-id="33183-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="33183-249">Selecione o **ProductId** coluna e o conjunto **ReadOnly** para **verdadeiro**.</span><span class="sxs-lookup"><span data-stu-id="33183-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="33183-250">Selecione o **CategoryId** coluna e pressione a **remover** botão.</span><span class="sxs-lookup"><span data-stu-id="33183-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="33183-251">Fazer o mesmo com o **categoria** coluna.</span><span class="sxs-lookup"><span data-stu-id="33183-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="33183-252">Pressione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="33183-252">Press **OK**.</span></span>

    <span data-ttu-id="33183-253">Até agora, podemos associados aos nossos controles DataGridView BindingSource componentes no designer.</span><span class="sxs-lookup"><span data-stu-id="33183-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="33183-254">A próxima seção, adicionaremos o código para code-behind definir categoryBindingSource.DataSource para a coleção de entidades que estão atualmente acompanhados pelo DbContext.</span><span class="sxs-lookup"><span data-stu-id="33183-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="33183-255">Quando estamos arrasta e solta os produtos de categoria, o WinForms cuidou de configurar a propriedade productsBindingSource.DataSource à propriedade categoryBindingSource e productsBindingSource.DataMember para produtos.</span><span class="sxs-lookup"><span data-stu-id="33183-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="33183-256">Devido a essa associação, somente os produtos que pertencem à categoria selecionada no momento serão exibidos no productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="33183-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="33183-257">Habilitar o **salve** botão na barra de navegação clicando o botão direito do mouse e selecionando **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="33183-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Designer do Form1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="33183-259">Adicione o manipulador de eventos para o salvamento botão clicando duas vezes no botão.</span><span class="sxs-lookup"><span data-stu-id="33183-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="33183-260">Isso adicionará o manipulador de eventos e levam você para o code-behind para o formulário.</span><span class="sxs-lookup"><span data-stu-id="33183-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="33183-261">O código para o **categoryBindingNavigatorSaveItem\_clique em** manipulador de eventos será adicionado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="33183-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="33183-262">Adicione o código que trata a interação de dados</span><span class="sxs-lookup"><span data-stu-id="33183-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="33183-263">Agora, adicionaremos o código para usar o ProductContext para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="33183-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="33183-264">Atualize o código para a janela do formulário principal, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="33183-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="33183-265">O código declara uma instância de execução longa da ProductContext.</span><span class="sxs-lookup"><span data-stu-id="33183-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="33183-266">O objeto ProductContext é usado para consultar e salvar dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33183-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="33183-267">O método Dispose () na instância ProductContext, em seguida, é chamado de método OnClosing substituído.</span><span class="sxs-lookup"><span data-stu-id="33183-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="33183-268">Os comentários do código fornecem detalhes sobre o que o código faz.</span><span class="sxs-lookup"><span data-stu-id="33183-268">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="33183-269">Testar o aplicativo do Windows Forms</span><span class="sxs-lookup"><span data-stu-id="33183-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="33183-270">Compile e execute o aplicativo e você pode testar a funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="33183-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="33183-272">Depois de salvar as chaves geradas pelo repositório são mostradas na tela.</span><span class="sxs-lookup"><span data-stu-id="33183-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="33183-274">Se você usou o Code First e, em seguida, você também verá que um **WinFormswithEFSample.ProductContext** banco de dados é criado para você.</span><span class="sxs-lookup"><span data-stu-id="33183-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)
