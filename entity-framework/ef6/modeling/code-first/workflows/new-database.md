---
title: Code First a um novo banco de dados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182569"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="90969-102">Code First a um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="90969-102">Code First to a New Database</span></span>
<span data-ttu-id="90969-103">Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Code First visando um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="90969-104">Esse cenário inclui o direcionamento de um banco de dados que não existe e Code First será criado, ou um banco de dados vazio ao qual Code First adicionará novas tabelas.</span><span class="sxs-lookup"><span data-stu-id="90969-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="90969-105">Code First permite que você defina seu modelo usando classes C @ no__t-0 ou VB.Net.</span><span class="sxs-lookup"><span data-stu-id="90969-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="90969-106">A configuração adicional pode, opcionalmente, ser executada usando atributos em suas classes e propriedades ou usando uma API fluente.</span><span class="sxs-lookup"><span data-stu-id="90969-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="90969-107">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="90969-107">Watch the video</span></span>
<span data-ttu-id="90969-108">Este vídeo fornece uma introdução ao desenvolvimento de Code First destinado a um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="90969-109">Esse cenário inclui o direcionamento de um banco de dados que não existe e Code First será criado, ou um banco de dados vazio ao qual Code First adicionará novas tabelas.</span><span class="sxs-lookup"><span data-stu-id="90969-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="90969-110">Code First permite que você defina seu modelo usando C# as classes ou VB.net.</span><span class="sxs-lookup"><span data-stu-id="90969-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="90969-111">A configuração adicional pode, opcionalmente, ser executada usando atributos em suas classes e propriedades ou usando uma API fluente.</span><span class="sxs-lookup"><span data-stu-id="90969-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="90969-112">**Apresentado por**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="90969-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="90969-113">**Vídeo**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="90969-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="90969-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="90969-114">Pre-Requisites</span></span>

<span data-ttu-id="90969-115">Você precisará ter, pelo menos, o Visual Studio 2010 ou o Visual Studio 2012 instalado para concluir esta explicação.</span><span class="sxs-lookup"><span data-stu-id="90969-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="90969-116">Se você estiver usando o Visual Studio 2010, também será necessário ter o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="90969-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="90969-117">1. Criar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="90969-117">1. Create the Application</span></span>

<span data-ttu-id="90969-118">Para manter as coisas simples, vamos criar um aplicativo de console básico que usa Code First para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="90969-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="90969-119">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90969-119">Open Visual Studio</span></span>
-   <span data-ttu-id="90969-120">**Arquivo-&gt; Projeto New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="90969-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="90969-121">Selecione **Windows** no menu à esquerda e no **aplicativo de console**</span><span class="sxs-lookup"><span data-stu-id="90969-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="90969-122">Insira **CodeFirstNewDatabaseSample** como o nome</span><span class="sxs-lookup"><span data-stu-id="90969-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="90969-123">Selecione **OK**</span><span class="sxs-lookup"><span data-stu-id="90969-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="90969-124">2. Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="90969-124">2. Create the Model</span></span>

<span data-ttu-id="90969-125">Vamos definir um modelo muito simples usando classes.</span><span class="sxs-lookup"><span data-stu-id="90969-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="90969-126">Estamos apenas definindo-as no arquivo Program.cs, mas em um aplicativo real, você dividiria suas classes em arquivos separados e potencialmente um projeto separado.</span><span class="sxs-lookup"><span data-stu-id="90969-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="90969-127">Abaixo da definição de classe do programa em Program.cs, adicione as duas classes a seguir.</span><span class="sxs-lookup"><span data-stu-id="90969-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="90969-128">Você observará que estamos fazendo as duas propriedades de navegação (blog. postes e post. blog) virtuais.</span><span class="sxs-lookup"><span data-stu-id="90969-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="90969-129">Isso habilita o recurso de carregamento lento do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="90969-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="90969-130">O carregamento lento significa que o conteúdo dessas propriedades será carregado automaticamente do banco de dados quando você tentar acessá-los.</span><span class="sxs-lookup"><span data-stu-id="90969-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="90969-131">3. Criar um contexto</span><span class="sxs-lookup"><span data-stu-id="90969-131">3. Create a Context</span></span>

<span data-ttu-id="90969-132">Agora, é hora de definir um contexto derivado, que representa uma sessão com o banco de dados, permitindo que possamos consultar e salvar.</span><span class="sxs-lookup"><span data-stu-id="90969-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="90969-133">Definimos um contexto que deriva de System. Data. Entity. DbContext e expõe um tipo DbSet @ no__t-0TEntity @ no__t-1 para cada classe em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="90969-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="90969-134">Agora estamos começando a usar os tipos do Entity Framework, portanto, precisamos adicionar o pacote NuGet do EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="90969-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="90969-135">**Projeto – &gt; gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="90969-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="90969-136">Observação: Se você não tiver o **Manage NuGet Packages...**</span><span class="sxs-lookup"><span data-stu-id="90969-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="90969-137">opção você deve instalar a [versão mais recente do NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="90969-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="90969-138">Selecione a guia **online**</span><span class="sxs-lookup"><span data-stu-id="90969-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="90969-139">Selecionar o pacote do **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="90969-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="90969-140">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="90969-140">Click **Install**</span></span>

<span data-ttu-id="90969-141">Adicione uma instrução using para System. Data. Entity na parte superior de Program.cs.</span><span class="sxs-lookup"><span data-stu-id="90969-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="90969-142">Abaixo da classe post em Program.cs, adicione o seguinte contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="90969-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="90969-143">Aqui está uma lista completa do que o Program.cs deve conter agora.</span><span class="sxs-lookup"><span data-stu-id="90969-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="90969-144">Esse é todo o código de que precisamos para iniciar o armazenamento e a recuperação de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="90969-145">Obviamente, há um pouco sobre os bastidores e vamos dar uma olhada nele em um momento, mas primeiro vamos vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="90969-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="90969-146">4. Lendo & gravando dados</span><span class="sxs-lookup"><span data-stu-id="90969-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="90969-147">Implemente o método Main em Program.cs, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="90969-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="90969-148">Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo blog.</span><span class="sxs-lookup"><span data-stu-id="90969-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="90969-149">Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados ordenado alfabeticamente por título.</span><span class="sxs-lookup"><span data-stu-id="90969-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="90969-150">Agora você pode executar o aplicativo e testá-lo.</span><span class="sxs-lookup"><span data-stu-id="90969-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="90969-151">Onde estão meus dados?</span><span class="sxs-lookup"><span data-stu-id="90969-151">Where’s My Data?</span></span>

<span data-ttu-id="90969-152">Por convenção DbContext criou um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="90969-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="90969-153">Se uma instância local do SQL Express estiver disponível (instalada por padrão com o Visual Studio 2010), Code First criou o banco de dados nessa instância</span><span class="sxs-lookup"><span data-stu-id="90969-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="90969-154">Se o SQL Express não estiver disponível, Code First tentará usar o [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalado por padrão com o Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="90969-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="90969-155">O banco de dados é nomeado após o nome totalmente qualificado do contexto derivado, em nosso caso, é **CodeFirstNewDatabaseSample. BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="90969-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="90969-156">Essas são apenas as convenções padrão e há várias maneiras de alterar o banco de dados que Code First usa, mais informações estão disponíveis no tópico **como o DbContext descobre o modelo e a conexão de banco de dados** .</span><span class="sxs-lookup"><span data-stu-id="90969-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="90969-157">Você pode se conectar a este banco de dados usando Gerenciador de Servidores no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90969-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="90969-158">**Exibir-&gt; Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="90969-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="90969-159">Clique com o botão direito em **conexões de dados** e selecione **Adicionar conexão...**</span><span class="sxs-lookup"><span data-stu-id="90969-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="90969-160">Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado</span><span class="sxs-lookup"><span data-stu-id="90969-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="90969-162">Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou</span><span class="sxs-lookup"><span data-stu-id="90969-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="90969-163">Agora podemos inspecionar o esquema que Code First criado.</span><span class="sxs-lookup"><span data-stu-id="90969-163">We can now inspect the schema that Code First created.</span></span>

![Inicial do esquema](~/ef6/media/schemainitial.png)

<span data-ttu-id="90969-165">O DbContext deu as classes a serem incluídas no modelo examinando as propriedades DbSet que definimos.</span><span class="sxs-lookup"><span data-stu-id="90969-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="90969-166">Em seguida, ele usa o conjunto padrão de convenções de Code First para determinar nomes de tabela e coluna, determinar tipos de dados, localizar chaves primárias, etc.</span><span class="sxs-lookup"><span data-stu-id="90969-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="90969-167">Mais adiante neste tutorial, veremos como você pode substituir essas convenções.</span><span class="sxs-lookup"><span data-stu-id="90969-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="90969-168">5. Lidando com alterações de modelo</span><span class="sxs-lookup"><span data-stu-id="90969-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="90969-169">Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações, também precisamos atualizar o esquema do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="90969-170">Para isso, vamos usar um recurso chamado Migrações do Code First ou migrações de forma abreviada.</span><span class="sxs-lookup"><span data-stu-id="90969-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="90969-171">As migrações nos permitem ter um conjunto ordenado de etapas que descrevem como atualizar (e fazer downgrade) nosso esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="90969-172">Cada uma dessas etapas, conhecida como migração, contém algum código que descreve as alterações a serem aplicadas.</span><span class="sxs-lookup"><span data-stu-id="90969-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="90969-173">A primeira etapa é habilitar Migrações do Code First para nosso BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="90969-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="90969-174">**Ferramentas-&gt;-Gerenciador de pacotes de biblioteca-&gt; console do Gerenciador de pacotes**</span><span class="sxs-lookup"><span data-stu-id="90969-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="90969-175">Execute o comando **Enable-Migrations** no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="90969-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="90969-176">Uma nova pasta de migrações foi adicionada ao nosso projeto que contém dois itens:</span><span class="sxs-lookup"><span data-stu-id="90969-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="90969-177">**Configuration.cs** – esse arquivo contém as configurações que serão usadas pelas migrações para a migração de BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="90969-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="90969-178">Não precisamos alterar nada para este passo a passos, mas aqui é onde você pode especificar dados de semente, registrar provedores para outros bancos de dado, altera o namespace em que as migrações são geradas etc.</span><span class="sxs-lookup"><span data-stu-id="90969-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="90969-179">**&lt;timestamp @ no__t-2\_InitialCreate.cs** – essa é a sua primeira migração, ela representa as alterações que já foram aplicadas ao banco de dados para levá-lo de ser um banco de dados vazio para um que inclua as tabelas Blogs e postagens.</span><span class="sxs-lookup"><span data-stu-id="90969-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="90969-180">Embora permitamos que Code First criem essas tabelas automaticamente para nós, agora que optamos por migrações que foram convertidas em uma migração.</span><span class="sxs-lookup"><span data-stu-id="90969-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="90969-181">Code First também foi registrado em nosso banco de dados local que essa migração já foi aplicada.</span><span class="sxs-lookup"><span data-stu-id="90969-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="90969-182">O carimbo de data/hora no nome de arquivo é usado para fins de ordenação.</span><span class="sxs-lookup"><span data-stu-id="90969-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="90969-183">Agora, vamos fazer uma alteração em nosso modelo, adicionar uma propriedade URL à classe blog:</span><span class="sxs-lookup"><span data-stu-id="90969-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="90969-184">Execute o comando **Add-Migration addurl** no console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="90969-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="90969-185">O comando Add-Migration verifica as alterações desde sua última migração e aplica Scaffold uma nova migração com as alterações encontradas.</span><span class="sxs-lookup"><span data-stu-id="90969-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="90969-186">Podemos dar um nome às migrações; Nesse caso, estamos chamando a migração ' AddUrl '.</span><span class="sxs-lookup"><span data-stu-id="90969-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="90969-187">O código com Scaffold está dizendo que precisamos adicionar uma coluna de URL, que pode conter dados de cadeia de caracteres, ao dbo. Tabela de Blogs.</span><span class="sxs-lookup"><span data-stu-id="90969-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="90969-188">Se necessário, poderíamos editar o código com Scaffold, mas isso não é necessário nesse caso.</span><span class="sxs-lookup"><span data-stu-id="90969-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="90969-189">Execute o comando **Update-Database** no console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="90969-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="90969-190">Esse comando aplicará todas as migrações pendentes ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="90969-191">Nossa migração de InitialCreate já foi aplicada, portanto, as migrações só aplicarão nossa nova migração AddUrl.</span><span class="sxs-lookup"><span data-stu-id="90969-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="90969-192">Dica: Você pode usar a opção **– Verbose** ao chamar Update-Database para ver o SQL que está sendo executado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="90969-193">A nova coluna URL agora é adicionada à tabela Blogs no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="90969-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Esquema com URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="90969-195">6. Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="90969-195">6. Data Annotations</span></span>

<span data-ttu-id="90969-196">Até agora, deixamos que o EF descubra o modelo usando suas convenções padrão, mas haverá momentos em que nossas classes não seguem as convenções e precisamos ser capazes de executar outras configurações.</span><span class="sxs-lookup"><span data-stu-id="90969-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="90969-197">Há duas opções para isso; Veremos as anotações de dados nesta seção e, em seguida, a API Fluent na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="90969-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="90969-198">Vamos adicionar uma classe de usuário ao nosso modelo</span><span class="sxs-lookup"><span data-stu-id="90969-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="90969-199">Também precisamos adicionar um conjunto ao nosso contexto derivado</span><span class="sxs-lookup"><span data-stu-id="90969-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="90969-200">Se tentamos adicionar uma migração, recebemos um erro informando que "o usuário" *EntityType ' não tem nenhuma chave definida. Defina a chave para este EntityType. "*</span><span class="sxs-lookup"><span data-stu-id="90969-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="90969-201">Porque o EF não tem como saber que username deve ser a chave primária para o usuário.</span><span class="sxs-lookup"><span data-stu-id="90969-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="90969-202">Vamos usar as anotações de dados agora para que seja necessário adicionar uma instrução using na parte superior de Program.cs</span><span class="sxs-lookup"><span data-stu-id="90969-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="90969-203">Agora, anote a propriedade UserName para identificar que é a chave primária</span><span class="sxs-lookup"><span data-stu-id="90969-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="90969-204">Use o comando **Add-migrar adduser** para Scaffold uma migração para aplicar essas alterações ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="90969-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="90969-205">Execute o comando **Update-Database** para aplicar a nova migração para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="90969-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="90969-206">A nova tabela agora é adicionada ao banco de dados:</span><span class="sxs-lookup"><span data-stu-id="90969-206">The new table is now added to the database:</span></span>

![Esquema com usuários](~/ef6/media/schemawithusers.png)

<span data-ttu-id="90969-208">A lista completa de anotações com suporte no EF é:</span><span class="sxs-lookup"><span data-stu-id="90969-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="90969-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="90969-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="90969-211">MaxLengthattribute</span><span class="sxs-lookup"><span data-stu-id="90969-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="90969-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="90969-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="90969-214">Carimbo de data/hora</span><span class="sxs-lookup"><span data-stu-id="90969-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="90969-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="90969-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="90969-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="90969-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="90969-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="90969-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="90969-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="90969-221">Não Mapeadoattribute</span><span class="sxs-lookup"><span data-stu-id="90969-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="90969-222">7. API fluente</span><span class="sxs-lookup"><span data-stu-id="90969-222">7. Fluent API</span></span>

<span data-ttu-id="90969-223">Na seção anterior, examinamos o uso de anotações de dados para complementar ou substituir o que foi detectado pela Convenção.</span><span class="sxs-lookup"><span data-stu-id="90969-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="90969-224">A outra maneira de configurar o modelo é por meio da API Fluent Code First.</span><span class="sxs-lookup"><span data-stu-id="90969-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="90969-225">A maioria das configurações de modelo pode ser feita usando anotações de dados simples.</span><span class="sxs-lookup"><span data-stu-id="90969-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="90969-226">A API fluente é uma maneira mais avançada de especificar a configuração do modelo que abrange tudo o que as anotações de dados podem fazer além de algumas configurações mais avançadas não possíveis com as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="90969-227">As anotações de dados e a API fluente podem ser usadas em conjunto.</span><span class="sxs-lookup"><span data-stu-id="90969-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="90969-228">Para acessar a API fluente, você substitui o método OnModelCreating em DbContext.</span><span class="sxs-lookup"><span data-stu-id="90969-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="90969-229">Digamos que queríamos renomear a coluna que User. DisplayName está armazenada para exibir @ no__t-0name.</span><span class="sxs-lookup"><span data-stu-id="90969-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="90969-230">Substitua o método OnModelCreating em BloggingContext pelo código a seguir</span><span class="sxs-lookup"><span data-stu-id="90969-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="90969-231">Use o comando **Add-Migration ChangeDisplayName** para Scaffold uma migração para aplicar essas alterações ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="90969-232">Execute o comando **Update-Database** para aplicar a nova migração para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="90969-233">A coluna DisplayName agora é renomeada para exibir @ no__t-0name:</span><span class="sxs-lookup"><span data-stu-id="90969-233">The DisplayName column is now renamed to display\_name:</span></span>

![Esquema com nome para exibição renomeado](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="90969-235">Resumo</span><span class="sxs-lookup"><span data-stu-id="90969-235">Summary</span></span>

<span data-ttu-id="90969-236">Neste tutorial, examinamos Code First desenvolvimento usando um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="90969-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="90969-237">Definimos um modelo usando classes e, em seguida, usamos esse modelo para criar um banco de dados e armazenar e recuperar os mesmos.</span><span class="sxs-lookup"><span data-stu-id="90969-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="90969-238">Depois que o banco de dados foi criado, usamos Migrações do Code First para alterar o esquema à medida que nosso modelo evoluiu.</span><span class="sxs-lookup"><span data-stu-id="90969-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="90969-239">Também vimos como configurar um modelo usando as anotações de dados e a API fluente.</span><span class="sxs-lookup"><span data-stu-id="90969-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
