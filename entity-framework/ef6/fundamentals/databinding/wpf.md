---
title: Vinculação de dados com WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639137"
---
> [!IMPORTANT]
> <span data-ttu-id="22ed5-102">**Este documento é válido apenas para WPF no Quadro .NET**</span><span class="sxs-lookup"><span data-stu-id="22ed5-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="22ed5-103">Este documento descreve a vinculação de dados para WPF no Quadro .NET.</span><span class="sxs-lookup"><span data-stu-id="22ed5-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="22ed5-104">Para novos projetos .NET Core, recomendamos que você use [o EF Core](/ef/core) em vez do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="22ed5-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="22ed5-105">A documentação para vinculação de dados no EF Core é rastreada no [#778 de Problemas](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span><span class="sxs-lookup"><span data-stu-id="22ed5-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="22ed5-106">Associação de dados com WPF</span><span class="sxs-lookup"><span data-stu-id="22ed5-106">Databinding with WPF</span></span>
<span data-ttu-id="22ed5-107">Este passo a passo mostra como vincular os tipos POCO aos controles WPF em um formulário "master-detail".</span><span class="sxs-lookup"><span data-stu-id="22ed5-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="22ed5-108">O aplicativo usa as APIs do Framework da entidade para preencher objetos com dados do banco de dados, rastrear alterações e persistir dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="22ed5-109">O modelo define dois tipos que participam de uma relação\\de um a muitos: **Categoria** (master principal) e **Produto** (detalhe dependente).\\</span><span class="sxs-lookup"><span data-stu-id="22ed5-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="22ed5-110">Em seguida, as ferramentas do Visual Studio são usadas para vincular os tipos definidos no modelo aos controles WPF.</span><span class="sxs-lookup"><span data-stu-id="22ed5-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="22ed5-111">A estrutura de vinculação de dados do WPF permite a navegação entre objetos relacionados: selecionar linhas na exibição mestre faz com que a visualização de detalhes seja atualizada com os dados de filho correspondentes.</span><span class="sxs-lookup"><span data-stu-id="22ed5-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="22ed5-112">As capturas de tela e as listas de código neste passo a passo são tiradas do Visual Studio 2013, mas você pode completar este passo a passo com o Visual Studio 2012 ou visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="22ed5-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="22ed5-113">Use a opção 'Objeto' para criar fontes de dados WPF</span><span class="sxs-lookup"><span data-stu-id="22ed5-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="22ed5-114">Com a versão anterior do Entity Framework, costumávamos recomendar o uso da opção **Banco de Dados** ao criar uma nova Fonte de Dados baseada em um modelo criado com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="22ed5-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="22ed5-115">Isso porque o designer geraria um contexto derivado do ObjectContext e das classes de entidade susobtidas derivadas do EntityObject.</span><span class="sxs-lookup"><span data-stu-id="22ed5-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="22ed5-116">Usar a opção Banco de Dados ajudaria a escrever o melhor código para interagir com essa superfície de API.</span><span class="sxs-lookup"><span data-stu-id="22ed5-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="22ed5-117">Os EF Designers for Visual Studio 2012 e Visual Studio 2013 geram um contexto que deriva do DbContext juntamente com classes simples de entidadePOCO.</span><span class="sxs-lookup"><span data-stu-id="22ed5-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="22ed5-118">Com o Visual Studio 2010 recomendamos a troca por um modelo de geração de código que usa o DbContext como descrito mais tarde neste passo a passo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="22ed5-119">Ao usar a superfície da API dbContext, você deve usar a opção **Objeto** ao criar uma nova Fonte de Dados, como mostrado neste passo a passo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="22ed5-120">Se necessário, você pode [reverter para a geração de código baseado em ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para modelos criados com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="22ed5-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="22ed5-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="22ed5-121">Pre-Requisites</span></span>

<span data-ttu-id="22ed5-122">Você precisa ter o Visual Studio 2013, visual studio 2012 ou Visual Studio 2010 instalado para completar este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="22ed5-123">Se você estiver usando o Visual Studio 2010, você também tem que instalar o NuGet.</span><span class="sxs-lookup"><span data-stu-id="22ed5-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="22ed5-124">Para obter mais informações, consulte [Instalando NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="22ed5-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="22ed5-125">Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="22ed5-125">Create the Application</span></span>

-   <span data-ttu-id="22ed5-126">Abra o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22ed5-126">Open Visual Studio</span></span>
-   <span data-ttu-id="22ed5-127">**Arquivo&gt; -&gt; Novo - Projeto....**</span><span class="sxs-lookup"><span data-stu-id="22ed5-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="22ed5-128">Selecione **Windows** no painel esquerdo e **WPFApplication** no painel direito</span><span class="sxs-lookup"><span data-stu-id="22ed5-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="22ed5-129">Digite **WPFwithEFSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="22ed5-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="22ed5-130">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="22ed5-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="22ed5-131">Instale o pacote NuGet do Framework da entidade</span><span class="sxs-lookup"><span data-stu-id="22ed5-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="22ed5-132">No Solution Explorer, clique com o botão direito do mouse no projeto **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="22ed5-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="22ed5-133">Selecione **Gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="22ed5-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="22ed5-134">Na caixa de diálogo Gerenciar pacotes NuGet, selecione a guia **On-line** e escolha o pacote **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="22ed5-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="22ed5-135">Clique **em Instalar**</span><span class="sxs-lookup"><span data-stu-id="22ed5-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="22ed5-136">Além do conjunto EntityFramework, também é adicionada uma referência ao System.ComponentModel.DataAnnotações.</span><span class="sxs-lookup"><span data-stu-id="22ed5-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="22ed5-137">Se o projeto tiver uma referência ao System.Data.Entity, ele será removido quando o pacote EntityFramework estiver instalado.</span><span class="sxs-lookup"><span data-stu-id="22ed5-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="22ed5-138">O conjunto System.Data.Entity não é mais usado para aplicações do Quadro de Entidades 6.</span><span class="sxs-lookup"><span data-stu-id="22ed5-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="22ed5-139">Definir um modelo</span><span class="sxs-lookup"><span data-stu-id="22ed5-139">Define a Model</span></span>

<span data-ttu-id="22ed5-140">Neste passo a passo você pode optar por implementar um modelo usando o Code First ou o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="22ed5-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="22ed5-141">Complete uma das duas seções seguintes.</span><span class="sxs-lookup"><span data-stu-id="22ed5-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="22ed5-142">Opção 1: Defina um modelo usando o Código Primeiro</span><span class="sxs-lookup"><span data-stu-id="22ed5-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="22ed5-143">Esta seção mostra como criar um modelo e seu banco de dados associado usando o Code First.</span><span class="sxs-lookup"><span data-stu-id="22ed5-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="22ed5-144">Pule para a próxima seção **(Opção 2: Defina um modelo usando o banco de dados primeiro)** se você preferir usar o Banco de Dados Primeiro para fazer engenharia reversa do seu modelo a partir de um banco de dados usando o designer Da EF</span><span class="sxs-lookup"><span data-stu-id="22ed5-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="22ed5-145">Ao usar o desenvolvimento do Code First, você geralmente começa escrevendo classes .NET Framework que definem seu modelo conceitual (domínio).</span><span class="sxs-lookup"><span data-stu-id="22ed5-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="22ed5-146">Adicionar uma nova classe ao **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="22ed5-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="22ed5-147">Clique com o botão direito do mouse no nome do projeto</span><span class="sxs-lookup"><span data-stu-id="22ed5-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="22ed5-148">Selecione **Adicionar,** em seguida, **novo item**</span><span class="sxs-lookup"><span data-stu-id="22ed5-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="22ed5-149">Selecione **Classe** e digite **Produto** para o nome da classe</span><span class="sxs-lookup"><span data-stu-id="22ed5-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="22ed5-150">Substitua a definição de classe **de produto** pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="22ed5-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="22ed5-151">A **Products** propriedade Produtos **na** **categoria** classe e categoria de propriedade na classe **Produto** são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="22ed5-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="22ed5-152">No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar uma relação entre dois tipos de entidades.</span><span class="sxs-lookup"><span data-stu-id="22ed5-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="22ed5-153">Além de definir entidades, você precisa definir uma classe que deriva&lt;do&gt; DbContext e expõe as propriedades do DbSet TEntity.</span><span class="sxs-lookup"><span data-stu-id="22ed5-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="22ed5-154">As propriedades&lt;DbSet TEntity&gt; indizem o contexto para saber quais tipos você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="22ed5-155">Uma instância do tipo derivado do DbContext gerencia os objetos da entidade durante o tempo de execução, o que inclui preencher objetos com dados de um banco de dados, alterar o rastreamento e persistir dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="22ed5-156">Adicione uma nova classe **ProductContext** ao projeto com a seguinte definição:</span><span class="sxs-lookup"><span data-stu-id="22ed5-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="22ed5-157">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="22ed5-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="22ed5-158">Opção 2: Defina um modelo usando o banco de dados primeiro</span><span class="sxs-lookup"><span data-stu-id="22ed5-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="22ed5-159">Esta seção mostra como usar o Banco de Dados Primeiro para fazer engenharia reversa do seu modelo a partir de um banco de dados usando o designer EF.</span><span class="sxs-lookup"><span data-stu-id="22ed5-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="22ed5-160">Se você completou a seção anterior **(Opção 1: Defina um modelo usando o Código Primeiro)**, então pule esta seção e vá direto para a seção **Carga Preguiçosa.**</span><span class="sxs-lookup"><span data-stu-id="22ed5-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="22ed5-161">Criar um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="22ed5-161">Create an Existing Database</span></span>

<span data-ttu-id="22ed5-162">Normalmente, quando você está mirando em um banco de dados existente ele já será criado, mas para este passo a passo precisamos criar um banco de dados para acessar.</span><span class="sxs-lookup"><span data-stu-id="22ed5-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="22ed5-163">O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:</span><span class="sxs-lookup"><span data-stu-id="22ed5-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="22ed5-164">Se você estiver usando o Visual Studio 2010, você estará criando um banco de dados SQL Express.</span><span class="sxs-lookup"><span data-stu-id="22ed5-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="22ed5-165">Se você estiver usando o Visual Studio 2012, então você estará criando um banco de dados [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="22ed5-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="22ed5-166">Vamos em frente e gerar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="22ed5-167">**Ver&gt; - Explorador do servidor**</span><span class="sxs-lookup"><span data-stu-id="22ed5-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="22ed5-168">Clique com o botão direito do mouse em **Conexões de Dados -&gt; Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="22ed5-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="22ed5-169">Se você não tiver conectado a um banco de dados do Server Explorer antes de precisar selecionar o Microsoft SQL Server como fonte de dados</span><span class="sxs-lookup"><span data-stu-id="22ed5-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="22ed5-171">Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual você instalou e **insira Produtos** como o nome do banco de dados</span><span class="sxs-lookup"><span data-stu-id="22ed5-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Adicionar conexão LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar expresso de conexão](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="22ed5-174">Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**</span><span class="sxs-lookup"><span data-stu-id="22ed5-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="22ed5-176">O novo banco de dados agora aparecerá no Server Explorer, clique com o botão direito do mouse nele e selecione **Nova consulta**</span><span class="sxs-lookup"><span data-stu-id="22ed5-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="22ed5-177">Copie o SQL a seguir na nova consulta e clique com o botão direito do mouse na consulta e selecione **Executar**</span><span class="sxs-lookup"><span data-stu-id="22ed5-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="22ed5-178">Modelo de Engenheiro Reverso</span><span class="sxs-lookup"><span data-stu-id="22ed5-178">Reverse Engineer Model</span></span>

<span data-ttu-id="22ed5-179">Vamos usar o Entity Framework Designer, que está incluído como parte do Visual Studio, para criar nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="22ed5-180">**Projeto&gt; - Adicionar Novo Item...**</span><span class="sxs-lookup"><span data-stu-id="22ed5-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="22ed5-181">Selecione **Dados** no menu esquerdo e, em seguida, **ADO.NET Modelo de Dados da Entidade**</span><span class="sxs-lookup"><span data-stu-id="22ed5-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="22ed5-182">Digite **ProductModel** como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="22ed5-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="22ed5-183">Isso lança o **Assistente de Modelo de Dados de Entidade**</span><span class="sxs-lookup"><span data-stu-id="22ed5-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="22ed5-184">Selecione **Gerar no banco de dados** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="22ed5-184">Select **Generate from Database** and click **Next**</span></span>

    ![Escolher conteúdo do modelo](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="22ed5-186">Selecione a conexão com o banco de dados criado na primeira seção, digite **ProductContext** como o nome da seqüência de conexões e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="22ed5-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Escolha sua conexão](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="22ed5-188">Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'</span><span class="sxs-lookup"><span data-stu-id="22ed5-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Escolha seus objetos](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="22ed5-190">Uma vez que o processo de engenharia reversa é concluído, o novo modelo é adicionado ao seu projeto e aberto para você visualizar no Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="22ed5-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="22ed5-191">Um arquivo App.config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="22ed5-192">Passos adicionais no Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="22ed5-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="22ed5-193">Se você está trabalhando no Visual Studio 2010, então você precisará atualizar o designer Da EF para usar a geração de código EF6.</span><span class="sxs-lookup"><span data-stu-id="22ed5-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="22ed5-194">Clique com o botão direito do mouse em um local vazio do seu modelo no EF Designer e selecione **Adicionar item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="22ed5-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="22ed5-195">Selecione **Modelos Online** no menu esquerdo e pesquise por **DbContext**</span><span class="sxs-lookup"><span data-stu-id="22ed5-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="22ed5-196">Selecione o **Gerador EF\#6.x DbContext para C,** insira **ProductsModel** como nome e clique em Adicionar</span><span class="sxs-lookup"><span data-stu-id="22ed5-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="22ed5-197">Atualização da geração de código para vinculação de dados</span><span class="sxs-lookup"><span data-stu-id="22ed5-197">Updating code generation for data binding</span></span>

<span data-ttu-id="22ed5-198">EF gera código do seu modelo usando modelos T4.</span><span class="sxs-lookup"><span data-stu-id="22ed5-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="22ed5-199">Os modelos enviados com o Visual Studio ou baixados da galeria Visual Studio são destinados para uso geral.</span><span class="sxs-lookup"><span data-stu-id="22ed5-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="22ed5-200">Isso significa que as entidades geradas a&lt;&gt; partir desses modelos possuem propriedades simples de ICollection T.</span><span class="sxs-lookup"><span data-stu-id="22ed5-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="22ed5-201">No entanto, ao fazer a vinculação de dados usando o WPF é desejável usar **oObservávelCollection** para propriedades de coleta para que o WPF possa acompanhar as alterações feitas nas coleções.</span><span class="sxs-lookup"><span data-stu-id="22ed5-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="22ed5-202">Para isso, modificaremos os modelos para usar o ObservávelCollection.</span><span class="sxs-lookup"><span data-stu-id="22ed5-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="22ed5-203">Abra o **Solution Explorer** e encontre o arquivo **ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="22ed5-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="22ed5-204">Encontre o arquivo **ProductModel.tt** que será aninhado no arquivo ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="22ed5-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modelo de modelo de produto WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="22ed5-206">Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22ed5-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="22ed5-207">Encontre e substitua as duas ocorrências de "**ICollection**" por "**ObservávelCollection**".</span><span class="sxs-lookup"><span data-stu-id="22ed5-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="22ed5-208">Estes estão localizados aproximadamente nas linhas 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="22ed5-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="22ed5-209">Encontre e substitua a primeira ocorrência de "**HashSet**" com "**ObservávelCollection**".</span><span class="sxs-lookup"><span data-stu-id="22ed5-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="22ed5-210">Esta ocorrência está localizada aproximadamente na linha 50.</span><span class="sxs-lookup"><span data-stu-id="22ed5-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="22ed5-211">**Não** substitua a segunda ocorrência de HashSet encontrada posteriormente no código.</span><span class="sxs-lookup"><span data-stu-id="22ed5-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="22ed5-212">Encontre e substitua a única ocorrência de "**System.Collections.Generic**" com "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="22ed5-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="22ed5-213">Isto está localizado aproximadamente na linha 424.</span><span class="sxs-lookup"><span data-stu-id="22ed5-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="22ed5-214">Salve o arquivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="22ed5-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="22ed5-215">Isso deve fazer com que o código para entidades seja regenerado.</span><span class="sxs-lookup"><span data-stu-id="22ed5-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="22ed5-216">Se o código não se regenerar automaticamente, clique com o botão direito do mouse em ProductModel.tt e escolha "Executar ferramenta personalizada".</span><span class="sxs-lookup"><span data-stu-id="22ed5-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="22ed5-217">Se você abrir agora o arquivo Category.cs (que está aninhado em ProductModel.tt) então você deve ver que a coleção Produtos tem o tipo **Produto&lt;&gt;de Coleta Observável**.</span><span class="sxs-lookup"><span data-stu-id="22ed5-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="22ed5-218">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="22ed5-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="22ed5-219">Carregamento preguiçoso</span><span class="sxs-lookup"><span data-stu-id="22ed5-219">Lazy Loading</span></span>

<span data-ttu-id="22ed5-220">A **Products** propriedade Produtos **na** **categoria** classe e categoria de propriedade na classe **Produto** são propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="22ed5-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="22ed5-221">No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar uma relação entre dois tipos de entidades.</span><span class="sxs-lookup"><span data-stu-id="22ed5-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="22ed5-222">A EF oferece uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="22ed5-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="22ed5-223">Com esse tipo de carregamento (chamado de carregamento preguiçoso), saiba que na primeira vez que você acessar cada propriedade de navegação uma consulta separada será executada no banco de dados se o conteúdo ainda não estiver no contexto.</span><span class="sxs-lookup"><span data-stu-id="22ed5-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="22ed5-224">Ao usar os tipos de entidade POCO, o EF consegue um carregamento preguiçoso criando instâncias de tipos proxy derivados durante o tempo de execução e, em seguida, substituindo propriedades virtuais em suas classes para adicionar o gancho de carregamento.</span><span class="sxs-lookup"><span data-stu-id="22ed5-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="22ed5-225">Para obter o carregamento preguiçoso de objetos relacionados, você deve declarar os getters de propriedade de navegação como **públicos** e **virtuais** (**Overridable** in Visual Basic), e você classe não deve ser **lacrado** **(NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="22ed5-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="22ed5-226">Ao usar o Banco de Dados, as propriedades de navegação First são automaticamente feitas virtuais para permitir o carregamento preguiçoso.</span><span class="sxs-lookup"><span data-stu-id="22ed5-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="22ed5-227">Na seção Código Primeiro, optamos por tornar as propriedades de navegação virtuais pelo mesmo motivo</span><span class="sxs-lookup"><span data-stu-id="22ed5-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="22ed5-228">Vincular objeto a controles</span><span class="sxs-lookup"><span data-stu-id="22ed5-228">Bind Object to Controls</span></span>

<span data-ttu-id="22ed5-229">Adicione as classes definidas no modelo como fontes de dados para este aplicativo WPF.</span><span class="sxs-lookup"><span data-stu-id="22ed5-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="22ed5-230">Clique duas vezes em **MainWindow.xaml** no Solution Explorer para abrir o formulário principal</span><span class="sxs-lookup"><span data-stu-id="22ed5-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="22ed5-231">No menu principal, selecione **Projeto -&gt; Adicionar Nova Fonte de Dados ...**</span><span class="sxs-lookup"><span data-stu-id="22ed5-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="22ed5-232">(no Visual Studio 2010, você precisa selecionar **Dados&gt; - Adicionar Nova Fonte de Dados...**)</span><span class="sxs-lookup"><span data-stu-id="22ed5-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="22ed5-233">Na janela Escolher uma janela de tipo de origem de dados, selecione **Objeto** e clique **em Seguir**</span><span class="sxs-lookup"><span data-stu-id="22ed5-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="22ed5-234">Na marca Select the Data Objects (Seleção objetos de dados) desdobre o **WPFwithEFSample** duas vezes e selecione **Categoria**</span><span class="sxs-lookup"><span data-stu-id="22ed5-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="22ed5-235">*Não há necessidade de selecionar a fonte de dados do **Produto,** porque chegaremos a ela através da propriedade do **Produto**na fonte de dados **da Categoria***</span><span class="sxs-lookup"><span data-stu-id="22ed5-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Selecionar objetos de dados](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="22ed5-237">Clique em **Concluir.**</span><span class="sxs-lookup"><span data-stu-id="22ed5-237">Click **Finish.**</span></span>
-   <span data-ttu-id="22ed5-238">A janela Fontes de dados é aberta ao lado da janela MainWindow.xaml \*Se a janela Fontes de dados não estiver aparecendo, selecione Exibir - **&gt; Outras fontes de&gt; dados do Windows** \*</span><span class="sxs-lookup"><span data-stu-id="22ed5-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="22ed5-239">Pressione o ícone do pino, para que a janela Fontes de dados não se esconda automaticamente.</span><span class="sxs-lookup"><span data-stu-id="22ed5-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="22ed5-240">Talvez seja necessário apertar o botão de atualização se a janela já estiver visível.</span><span class="sxs-lookup"><span data-stu-id="22ed5-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Fontes de dados](~/ef6/media/datasources.png)

-   <span data-ttu-id="22ed5-242">Selecione a fonte de dados **da categoria** e arraste-a no formulário.</span><span class="sxs-lookup"><span data-stu-id="22ed5-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="22ed5-243">O seguinte aconteceu quando arrastamos esta fonte:</span><span class="sxs-lookup"><span data-stu-id="22ed5-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="22ed5-244">O recurso **categoryViewSource** e a **categoriaControleDataGrid** foram adicionados ao XAML</span><span class="sxs-lookup"><span data-stu-id="22ed5-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="22ed5-245">A propriedade DataContext no elemento Grade pai foi definida como "{Categoria **StaticResourceViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="22ed5-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="22ed5-246">O recurso **categoryViewSource** serve como uma\\fonte de vinculação para o elemento de grade pai externo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="22ed5-247">Os elementos internos da Grade herdam o valor DataContext da grade pai (a propriedade ItemsSource da categoriaDataGrid está definida como "{Binding}")</span><span class="sxs-lookup"><span data-stu-id="22ed5-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="22ed5-248">Adicionando uma grade de detalhes</span><span class="sxs-lookup"><span data-stu-id="22ed5-248">Adding a Details Grid</span></span>

<span data-ttu-id="22ed5-249">Agora que temos uma grade para exibir Categorias, vamos adicionar uma grade de detalhes para exibir os Produtos associados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="22ed5-250">Selecione a propriedade **Produtos** na fonte de dados **da Categoria** e arraste-a no formulário.</span><span class="sxs-lookup"><span data-stu-id="22ed5-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="22ed5-251">A **categoriaProductsViewSource** recurso e **produtoDataGrid** grade são adicionados ao XAML</span><span class="sxs-lookup"><span data-stu-id="22ed5-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="22ed5-252">O caminho de vinculação para este recurso é definido como Produtos</span><span class="sxs-lookup"><span data-stu-id="22ed5-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="22ed5-253">A estrutura de vinculação de dados do WPF garante que apenas produtos relacionados à categoria selecionada apareçam no **produtoDataGrid**</span><span class="sxs-lookup"><span data-stu-id="22ed5-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="22ed5-254">Na caixa de ferramentas, arraste **Button** para o formulário.</span><span class="sxs-lookup"><span data-stu-id="22ed5-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="22ed5-255">Defina a propriedade **Nome** para **botãoSalvar** e a propriedade **Conteúdo** para **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="22ed5-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="22ed5-256">O formulário deve ser semelhante a isso:</span><span class="sxs-lookup"><span data-stu-id="22ed5-256">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="22ed5-258">Adicionar código que lida com a interação de dados</span><span class="sxs-lookup"><span data-stu-id="22ed5-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="22ed5-259">É hora de adicionar alguns manipuladores de eventos à janela principal.</span><span class="sxs-lookup"><span data-stu-id="22ed5-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="22ed5-260">Na janela XAML, clique \*\* &lt;\*\* no elemento Janela, isso seleciona a janela principal</span><span class="sxs-lookup"><span data-stu-id="22ed5-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="22ed5-261">Na janela **Propriedades,** escolha **Eventos** no canto superior direito e clique duas vezes na caixa de texto para a direita do rótulo **Carregado**</span><span class="sxs-lookup"><span data-stu-id="22ed5-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Propriedades da janela principal](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="22ed5-263">Adicione também o evento **Clique** para salvar o botão **Salvar** clicando duas vezes no botão Salvar no designer.</span><span class="sxs-lookup"><span data-stu-id="22ed5-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="22ed5-264">Isso o leva ao código para trás para o formulário, agora vamos editar o código para usar o ProductContext para realizar o acesso aos dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="22ed5-265">Atualize o código para o MainWindow como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="22ed5-266">O código declara uma instância de longo prazo do **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="22ed5-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="22ed5-267">O objeto **ProductContext** é usado para consultar e salvar dados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="22ed5-268">**A ocorrência Descarte()** na instância **ProductContext** é então chamada do método **OnClosing** substituído.</span><span class="sxs-lookup"><span data-stu-id="22ed5-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="22ed5-269">Os comentários do código fornecem detalhes sobre o que o código faz.</span><span class="sxs-lookup"><span data-stu-id="22ed5-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="22ed5-270">Teste o aplicativo WPF</span><span class="sxs-lookup"><span data-stu-id="22ed5-270">Test the WPF Application</span></span>

-   <span data-ttu-id="22ed5-271">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22ed5-271">Compile and run the application.</span></span> <span data-ttu-id="22ed5-272">Se você usou o Code First, verá que um banco de dados **WPFwithEFSample.ProductContext** foi criado para você.</span><span class="sxs-lookup"><span data-stu-id="22ed5-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="22ed5-273">Digite um nome de categoria na grade superior e nomes de produtos na grade inferior *Não digite nada em colunas de ID, pois a chave principal é gerada pelo banco de dados*</span><span class="sxs-lookup"><span data-stu-id="22ed5-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Janela Principal com novas categorias e produtos](~/ef6/media/screen1.png)

-   <span data-ttu-id="22ed5-275">Pressione o botão **Salvar** para salvar os dados no banco de dados</span><span class="sxs-lookup"><span data-stu-id="22ed5-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="22ed5-276">Após a chamada para **SaveChanges()** do DbContext, os IDs são preenchidos com os valores gerados pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22ed5-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="22ed5-277">Como chamamos **de Atualização()** após **saveChanges()** os controles **do DataGrid** também são atualizados com os novos valores.</span><span class="sxs-lookup"><span data-stu-id="22ed5-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Janela principal com IDs preenchidos](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="22ed5-279">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="22ed5-279">Additional Resources</span></span>

<span data-ttu-id="22ed5-280">Para saber mais sobre a vinculação de dados às coletas usando o WPF, consulte [este tópico](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) na documentação do WPF.</span><span class="sxs-lookup"><span data-stu-id="22ed5-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
