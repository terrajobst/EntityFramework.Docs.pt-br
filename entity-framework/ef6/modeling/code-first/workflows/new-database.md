---
title: Code First para um novo banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251044"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="3e451-102">Code First para um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="3e451-102">Code First to a New Database</span></span>
<span data-ttu-id="3e451-103">Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Code First, direcionando um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="3e451-104">Este cenário inclui o direcionamento de um banco de dados que não existe e Code First criará ou um banco de dados vazio que Code First será adicionar novas tabelas.</span><span class="sxs-lookup"><span data-stu-id="3e451-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="3e451-105">Código primeiro permite que você defina seu modelo usando o C\# ou classes VB.Net.</span><span class="sxs-lookup"><span data-stu-id="3e451-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="3e451-106">Configuração adicional, opcionalmente, pode ser executada usando atributos em classes e propriedades ou usando uma API fluente.</span><span class="sxs-lookup"><span data-stu-id="3e451-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3e451-107">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="3e451-107">Watch the video</span></span>
<span data-ttu-id="3e451-108">Este vídeo fornece uma introdução ao desenvolvimento Code First, direcionando um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="3e451-109">Este cenário inclui o direcionamento de um banco de dados que não existe e Code First criará ou um banco de dados vazio que Code First será adicionar novas tabelas.</span><span class="sxs-lookup"><span data-stu-id="3e451-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="3e451-110">Código primeiro permite que você defina seu modelo usando classes c# ou VB.Net.</span><span class="sxs-lookup"><span data-stu-id="3e451-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="3e451-111">Configuração adicional, opcionalmente, pode ser executada usando atributos em classes e propriedades ou usando uma API fluente.</span><span class="sxs-lookup"><span data-stu-id="3e451-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="3e451-112">**Apresentado por**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="3e451-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="3e451-113">**Vídeo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="3e451-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="3e451-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="3e451-114">Pre-Requisites</span></span>

<span data-ttu-id="3e451-115">Você precisará ter pelo menos o Visual Studio 2010 ou Visual Studio 2012 instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="3e451-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="3e451-116">Se você estiver usando o Visual Studio 2010, você também precisará ter [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="3e451-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="3e451-117">1. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="3e451-117">1. Create the Application</span></span>

<span data-ttu-id="3e451-118">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Code First para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="3e451-119">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e451-119">Open Visual Studio</span></span>
-   <span data-ttu-id="3e451-120">**Arquivo -&gt; New -&gt; projeto...**</span><span class="sxs-lookup"><span data-stu-id="3e451-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="3e451-121">Selecione **Windows** no menu à esquerda e **aplicativo de Console**</span><span class="sxs-lookup"><span data-stu-id="3e451-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="3e451-122">Insira **CodeFirstNewDatabaseSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="3e451-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="3e451-123">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="3e451-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="3e451-124">2. Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="3e451-124">2. Create the Model</span></span>

<span data-ttu-id="3e451-125">Vamos definir um modelo muito simples usando classes.</span><span class="sxs-lookup"><span data-stu-id="3e451-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="3e451-126">Estamos apenas definindo-os no arquivo Program.cs, mas em um aplicativo do mundo real que você seria dividir suas classes out em arquivos separados e, potencialmente, um projeto separado.</span><span class="sxs-lookup"><span data-stu-id="3e451-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="3e451-127">Abaixo da definição de classe do programa em Program.cs, adicione as seguintes classes.</span><span class="sxs-lookup"><span data-stu-id="3e451-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="3e451-128">Você observará que estamos fazendo as duas propriedades de navegação (Blog.Posts e Post.Blog) virtual.</span><span class="sxs-lookup"><span data-stu-id="3e451-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="3e451-129">Isso permite que o recurso de carregamento lento do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3e451-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="3e451-130">Carregamento lento significa que o conteúdo dessas propriedades serão automaticamente carregado do banco de dados quando você tentar acessá-los.</span><span class="sxs-lookup"><span data-stu-id="3e451-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="3e451-131">3. Criar um contexto</span><span class="sxs-lookup"><span data-stu-id="3e451-131">3. Create a Context</span></span>

<span data-ttu-id="3e451-132">Agora é hora de definir um contexto derivado que representa uma sessão com o banco de dados, que nos permite consultar e salvar dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="3e451-133">Definimos um contexto que deriva de System.Data.Entity.DbContext e expõe um DbSet tipado&lt;TEntity&gt; para cada classe em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="3e451-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="3e451-134">Agora estamos começando a usar tipos do Entity Framework, portanto, precisamos adicionar o pacote EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="3e451-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="3e451-135">**Projeto –&gt; gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="3e451-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="3e451-136">Observação: Se você não tiver o **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="3e451-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="3e451-137">opção, você deve instalar o [versão mais recente do NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="3e451-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="3e451-138">Selecione o **Online** guia</span><span class="sxs-lookup"><span data-stu-id="3e451-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="3e451-139">Selecione o **EntityFramework** pacote</span><span class="sxs-lookup"><span data-stu-id="3e451-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="3e451-140">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="3e451-140">Click **Install**</span></span>

<span data-ttu-id="3e451-141">Adicionar uma instrução de Entity na parte superior de Program.cs.</span><span class="sxs-lookup"><span data-stu-id="3e451-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="3e451-142">Abaixo a classe de postagem em Program.cs, adicione o seguinte contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="3e451-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="3e451-143">Aqui está uma listagem completa dos quais Program.cs agora deve conter.</span><span class="sxs-lookup"><span data-stu-id="3e451-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="3e451-144">Isso é todo o código que é preciso começar a armazenar e recuperar dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="3e451-145">É claro que há bem pouco acontecendo nos bastidores e vamos dar uma olhada no que em breve, mas primeiro vamos ver isso em ação.</span><span class="sxs-lookup"><span data-stu-id="3e451-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="3e451-146">4. Ler e gravar dados</span><span class="sxs-lookup"><span data-stu-id="3e451-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="3e451-147">Implemente o método Main em Program.cs, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3e451-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="3e451-148">Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir um novo Blog.</span><span class="sxs-lookup"><span data-stu-id="3e451-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="3e451-149">Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados classificado em ordem alfabética por título.</span><span class="sxs-lookup"><span data-stu-id="3e451-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="3e451-150">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="3e451-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="3e451-151">Onde estão meus dados?</span><span class="sxs-lookup"><span data-stu-id="3e451-151">Where’s My Data?</span></span>

<span data-ttu-id="3e451-152">Por convenção, o DbContext criou um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="3e451-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="3e451-153">Se uma instância local do SQL Express está disponível (instalado por padrão com o Visual Studio 2010), em seguida, Code First criou o banco de dados nessa instância</span><span class="sxs-lookup"><span data-stu-id="3e451-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="3e451-154">Se o SQL Express não está disponível, Code First será tente e use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalado por padrão com o Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="3e451-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="3e451-155">O banco de dados é nomeado após o nome totalmente qualificado do contexto derivado, em nosso caso, que é **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="3e451-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="3e451-156">Esses são apenas as convenções padrão e há várias maneiras de alterar o banco de dados que usa o Code First, mais informações estão disponíveis na **como o DbContext descobre o modelo e a Conexão de banco de dados** tópico.</span><span class="sxs-lookup"><span data-stu-id="3e451-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="3e451-157">Você pode se conectar ao banco de dados usando o Gerenciador de servidores no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3e451-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="3e451-158">**Exibição -&gt; Gerenciador de servidores**</span><span class="sxs-lookup"><span data-stu-id="3e451-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="3e451-159">Clique com botão direito **conexões de dados** e selecione **Adicionar Conexão...**</span><span class="sxs-lookup"><span data-stu-id="3e451-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="3e451-160">Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados</span><span class="sxs-lookup"><span data-stu-id="3e451-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="3e451-162">Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado</span><span class="sxs-lookup"><span data-stu-id="3e451-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="3e451-163">Agora podemos pode inspecionar o esquema que o código primeiro criou.</span><span class="sxs-lookup"><span data-stu-id="3e451-163">We can now inspect the schema that Code First created.</span></span>

![Esquema inicial](~/ef6/media/schemainitial.png)

<span data-ttu-id="3e451-165">DbContext solucionaram quais classes devem ser incluídas no modelo, observando as propriedades DbSet que definimos.</span><span class="sxs-lookup"><span data-stu-id="3e451-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="3e451-166">Em seguida, ele usa o conjunto padrão de convenções Code First para determinar os nomes de tabela e coluna, determinar os tipos de dados, localize as chaves primárias, etc.</span><span class="sxs-lookup"><span data-stu-id="3e451-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="3e451-167">Posteriormente neste passo a passo, examinaremos como você pode substituir essas convenções.</span><span class="sxs-lookup"><span data-stu-id="3e451-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="3e451-168">5. Lidando com alterações no modelo</span><span class="sxs-lookup"><span data-stu-id="3e451-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="3e451-169">Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações que também precisamos atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="3e451-170">Para fazer isso, vamos usar um recurso chamado migrações do Code First ou migrações de forma abreviada.</span><span class="sxs-lookup"><span data-stu-id="3e451-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="3e451-171">As migrações nos permite ter um conjunto ordenado de etapas que descrevem como atualizar (e fazer downgrade) nosso esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="3e451-172">Cada uma dessas etapas, conhecidas como uma migração, contém um código que descreve as alterações sejam aplicadas.</span><span class="sxs-lookup"><span data-stu-id="3e451-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="3e451-173">A primeira etapa é habilitar migrações do Code First para nosso BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="3e451-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="3e451-174">**Ferramentas -&gt; Gerenciador de pacotes de biblioteca -&gt; Package Manager Console**</span><span class="sxs-lookup"><span data-stu-id="3e451-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="3e451-175">Execute o comando **Enable-Migrations** no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="3e451-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="3e451-176">Uma nova pasta migrações foi adicionada ao nosso projeto que contém dois itens:</span><span class="sxs-lookup"><span data-stu-id="3e451-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="3e451-177">**Configuration.CS** – esse arquivo contém as configurações que usarão as migrações para a migração de BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="3e451-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="3e451-178">Não precisamos alterar nada para este passo a passo, mas aqui é onde você pode especificar dados de semente, provedores de registro para outros bancos de dados, altera o namespace que as migrações são geradas no etc.</span><span class="sxs-lookup"><span data-stu-id="3e451-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="3e451-179">**&lt;carimbo de hora&gt;\_InitialCreate.cs** – é sua primeira migração, ele representa as alterações que já foram aplicadas ao banco de dados para colocá-lo de ser um banco de dados vazio para um que inclua as tabelas de Blogs e postagens .</span><span class="sxs-lookup"><span data-stu-id="3e451-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="3e451-180">Embora nós deixava o Code First criar automaticamente essas tabelas para nós, agora que estamos tiver aceitado migrações foram convertidos em uma migração.</span><span class="sxs-lookup"><span data-stu-id="3e451-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="3e451-181">Código pela primeira vez também registrou em nosso banco de dados local se essa migração já foi aplicada.</span><span class="sxs-lookup"><span data-stu-id="3e451-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="3e451-182">O carimbo de hora em que o nome do arquivo é usado para fins de ordenação.</span><span class="sxs-lookup"><span data-stu-id="3e451-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="3e451-183">Agora vamos fazer uma alteração ao nosso modelo, adicione uma propriedade de Url para a classe de Blog:</span><span class="sxs-lookup"><span data-stu-id="3e451-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="3e451-184">Execute o **Add-Migration AddUrl** comando no Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="3e451-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="3e451-185">O comando Add-Migration procura as alterações desde a última migração e aplica Scaffold em uma nova migração com todas as alterações que são encontrados.</span><span class="sxs-lookup"><span data-stu-id="3e451-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="3e451-186">Pode dar a migrações de um nome; Nesse caso, chamamos a migração 'AddUrl'.</span><span class="sxs-lookup"><span data-stu-id="3e451-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="3e451-187">O código gerado por scaffolding está dizendo que precisamos adicionar uma coluna de Url, que pode conter dados de cadeia de caracteres, para o dbo. Tabela de blogs.</span><span class="sxs-lookup"><span data-stu-id="3e451-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="3e451-188">Se necessário, podemos pode editar o código gerado automaticamente, mas isso não é necessário neste caso.</span><span class="sxs-lookup"><span data-stu-id="3e451-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="3e451-189">Execute o **Update-Database** comando no Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="3e451-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="3e451-190">Esse comando irá aplicar quaisquer migrações pendentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="3e451-191">Nossa migração InitialCreate já foi aplicada para que migrações apenas seja aplicada a nossa nova migração AddUrl.</span><span class="sxs-lookup"><span data-stu-id="3e451-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="3e451-192">Dica: Você pode usar o **– Verbose** ao chamar Update-Database para ver o que está sendo executado no banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="3e451-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="3e451-193">A nova coluna de Url agora é adicionada à tabela Blogs no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="3e451-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Esquema de Url](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="3e451-195">6. Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="3e451-195">6. Data Annotations</span></span>

<span data-ttu-id="3e451-196">Até agora estamos apenas deixei o EF descobrir o modelo usando suas convenções de padrão, mas haverá momentos quando nossas classes não seguem as convenções e é preciso ser capaz de executar a configuração.</span><span class="sxs-lookup"><span data-stu-id="3e451-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="3e451-197">Há duas opções para isso. Vamos examinar as anotações de dados nesta seção e, em seguida, a API fluente na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="3e451-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="3e451-198">Vamos adicionar uma classe de usuário com o nosso modelo</span><span class="sxs-lookup"><span data-stu-id="3e451-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="3e451-199">Também precisamos adicionar um conjunto para o nosso contexto derivado</span><span class="sxs-lookup"><span data-stu-id="3e451-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="3e451-200">Se tentar adicionar uma migração, teríamos um erro dizendo que "*EntityType 'User' não tem nenhuma chave definida. Definir a chave para esse EntityType."*</span><span class="sxs-lookup"><span data-stu-id="3e451-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="3e451-201">porque o EF não tem como saber que o nome de usuário deve ser a chave primária para o usuário.</span><span class="sxs-lookup"><span data-stu-id="3e451-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="3e451-202">Vamos usar anotações de dados agora, portanto, precisamos adicionar uma instrução na parte superior de Program.cs</span><span class="sxs-lookup"><span data-stu-id="3e451-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="3e451-203">Agora, anote a propriedade de nome de usuário para identificar o que é a chave primária</span><span class="sxs-lookup"><span data-stu-id="3e451-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="3e451-204">Use o **Add-Migration AddUser** comando para criar o scaffolding de uma migração para aplicar essas alterações no banco de dados</span><span class="sxs-lookup"><span data-stu-id="3e451-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="3e451-205">Execute o **Update-Database** comando para aplicar a nova migração para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="3e451-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="3e451-206">Agora, a nova tabela é adicionada ao banco de dados:</span><span class="sxs-lookup"><span data-stu-id="3e451-206">The new table is now added to the database:</span></span>

![Esquema com usuários](~/ef6/media/schemawithusers.png)

<span data-ttu-id="3e451-208">A lista completa de anotações com suporte pelo EF é:</span><span class="sxs-lookup"><span data-stu-id="3e451-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="3e451-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="3e451-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="3e451-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="3e451-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="3e451-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="3e451-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="3e451-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="3e451-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="3e451-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="3e451-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="3e451-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="3e451-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="3e451-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="3e451-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="3e451-222">7. API fluente</span><span class="sxs-lookup"><span data-stu-id="3e451-222">7. Fluent API</span></span>

<span data-ttu-id="3e451-223">Na seção anterior, vimos usando Data Annotations para suplementar ou substituir o que foi detectado pela convenção.</span><span class="sxs-lookup"><span data-stu-id="3e451-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="3e451-224">A outra maneira para configurar o modelo é por meio da API fluente Code First.</span><span class="sxs-lookup"><span data-stu-id="3e451-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="3e451-225">A maioria das configuração de modelo pode ser feita usando anotações de dados simples.</span><span class="sxs-lookup"><span data-stu-id="3e451-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="3e451-226">A API fluente é uma maneira mais avançada de especificar a configuração de modelo que aborda tudo o que as anotações de dados podem fazer Além disso, alguns não são possíveis com anotações de dados de configuração mais avançado.</span><span class="sxs-lookup"><span data-stu-id="3e451-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="3e451-227">Anotações de dados e a API fluente podem ser usados juntos.</span><span class="sxs-lookup"><span data-stu-id="3e451-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="3e451-228">Para acessar a API fluente é substituir o método OnModelCreating no DbContext.</span><span class="sxs-lookup"><span data-stu-id="3e451-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="3e451-229">Digamos que queríamos renomear a coluna armazenados em User. DisplayName para exibir\_nome.</span><span class="sxs-lookup"><span data-stu-id="3e451-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="3e451-230">Substitua o método OnModelCreating no BloggingContext com o código a seguir</span><span class="sxs-lookup"><span data-stu-id="3e451-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="3e451-231">Use o **Add-Migration ChangeDisplayName** comando para criar o scaffolding de uma migração para aplicar essas alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="3e451-232">Execute o **Update-Database** comando para aplicar a nova migração para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="3e451-233">A coluna DisplayName agora foi renomeada para exibir\_nome:</span><span class="sxs-lookup"><span data-stu-id="3e451-233">The DisplayName column is now renamed to display\_name:</span></span>

![Esquema com nome de exibição renomeado](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="3e451-235">Resumo</span><span class="sxs-lookup"><span data-stu-id="3e451-235">Summary</span></span>

<span data-ttu-id="3e451-236">Neste passo a passo analisamos desenvolvimento Code First usando um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="3e451-237">Estamos definido um modelo usando classes e usado esse modelo para criar um banco de dados e armazenar e recuperar dados.</span><span class="sxs-lookup"><span data-stu-id="3e451-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="3e451-238">Depois que o banco de dados foi criado, nós usamos migrações do Code First para alterar o esquema como nosso modelo evoluído.</span><span class="sxs-lookup"><span data-stu-id="3e451-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="3e451-239">Também vimos como configurar um modelo usando Data Annotations e a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3e451-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
