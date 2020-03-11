---
title: Spatial-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419097"
---
# <a name="spatial---code-first"></a><span data-ttu-id="8494a-102">Code First espacial</span><span class="sxs-lookup"><span data-stu-id="8494a-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="8494a-103">**Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="8494a-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="8494a-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="8494a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="8494a-105">O vídeo e instruções passo a passo mostram como mapear tipos espaciais com Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="8494a-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="8494a-106">Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.</span><span class="sxs-lookup"><span data-stu-id="8494a-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="8494a-107">Este passo a passos usará Code First para criar um novo banco de dados, mas você também pode usar [Code First para um banco de dados existente](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="8494a-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="8494a-108">O suporte ao tipo espacial foi introduzido no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="8494a-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="8494a-109">Observe que para usar os novos recursos, como tipo espacial, enums e funções com valor de tabela, você deve direcionar .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="8494a-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="8494a-110">O Visual Studio 2012 visa o .NET 4,5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="8494a-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="8494a-111">Para usar os tipos de dados espaciais, você também deve usar um provedor de Entity Framework que tenha suporte espacial.</span><span class="sxs-lookup"><span data-stu-id="8494a-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="8494a-112">Consulte [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="8494a-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="8494a-113">Há dois tipos de dados espaciais principais: Geografia e Geometry.</span><span class="sxs-lookup"><span data-stu-id="8494a-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="8494a-114">O tipo de dados geography armazena dados dados elipsoidais (por exemplo, coordenadas de latitude e longitude de GPS).</span><span class="sxs-lookup"><span data-stu-id="8494a-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="8494a-115">O tipo de dados geometry representa o sistema de coordenadas euclidiana (simples).</span><span class="sxs-lookup"><span data-stu-id="8494a-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="8494a-116">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="8494a-116">Watch the video</span></span>
<span data-ttu-id="8494a-117">Este vídeo mostra como mapear tipos espaciais com Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="8494a-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="8494a-118">Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.</span><span class="sxs-lookup"><span data-stu-id="8494a-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="8494a-119">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="8494a-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="8494a-120">**Vídeo**: [wmv](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="8494a-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="8494a-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="8494a-121">Pre-Requisites</span></span>

<span data-ttu-id="8494a-122">Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.</span><span class="sxs-lookup"><span data-stu-id="8494a-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8494a-123">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="8494a-123">Set up the Project</span></span>

1.  <span data-ttu-id="8494a-124">Abrir o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8494a-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="8494a-125">No menu **arquivo** , aponte para **novo**e clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="8494a-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="8494a-126">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console**</span><span class="sxs-lookup"><span data-stu-id="8494a-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="8494a-127">Insira **SpatialCodeFirst** como o nome do projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="8494a-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="8494a-128">Definir um novo modelo usando Code First</span><span class="sxs-lookup"><span data-stu-id="8494a-128">Define a New Model using Code First</span></span>

<span data-ttu-id="8494a-129">Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).</span><span class="sxs-lookup"><span data-stu-id="8494a-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="8494a-130">O código a seguir define a classe University.</span><span class="sxs-lookup"><span data-stu-id="8494a-130">The code below defines the University class.</span></span>

<span data-ttu-id="8494a-131">A Universidade tem a propriedade Location do tipo DbGeography.</span><span class="sxs-lookup"><span data-stu-id="8494a-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="8494a-132">Para usar o tipo DbGeography, você deve adicionar uma referência ao assembly System. Data. Entity e também adicionar a instrução System. Data. espacial using.</span><span class="sxs-lookup"><span data-stu-id="8494a-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="8494a-133">Abra o arquivo Program.cs e cole o seguinte usando as instruções na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="8494a-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="8494a-134">Adicione a seguinte definição de classe de Universidade ao arquivo Program.cs.</span><span class="sxs-lookup"><span data-stu-id="8494a-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="8494a-135">Definir o tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="8494a-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="8494a-136">Além de definir entidades, você precisa definir uma classe derivada de DbContext e expõe DbSet&lt;TEntity&gt; Propriedades.</span><span class="sxs-lookup"><span data-stu-id="8494a-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="8494a-137">As propriedades DbSet&lt;TEntity&gt; permitem que o contexto saiba quais tipos você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="8494a-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="8494a-138">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="8494a-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="8494a-139">Os tipos DbContext e DbSet são definidos no assembly do EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="8494a-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="8494a-140">Adicionaremos uma referência a essa DLL usando o pacote NuGet do EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="8494a-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="8494a-141">Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto.</span><span class="sxs-lookup"><span data-stu-id="8494a-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="8494a-142">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="8494a-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="8494a-143">Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="8494a-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="8494a-144">Clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="8494a-144">Click **Install**</span></span>

<span data-ttu-id="8494a-145">Observe que, além do assembly do EntityFramework, uma referência ao assembly System. ComponentModel. Annotations também é adicionada.</span><span class="sxs-lookup"><span data-stu-id="8494a-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="8494a-146">Na parte superior do arquivo Program.cs, adicione a seguinte instrução Using:</span><span class="sxs-lookup"><span data-stu-id="8494a-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="8494a-147">No Program.cs, adicione a definição de contexto.</span><span class="sxs-lookup"><span data-stu-id="8494a-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="8494a-148">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="8494a-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="8494a-149">Abra o arquivo Program.cs em que o método Main está definido.</span><span class="sxs-lookup"><span data-stu-id="8494a-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="8494a-150">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="8494a-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="8494a-151">O código adiciona dois novos objetos University ao contexto.</span><span class="sxs-lookup"><span data-stu-id="8494a-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="8494a-152">As propriedades espaciais são inicializadas usando o método DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="8494a-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="8494a-153">O ponto de Geografia representado como WellKnownText é passado para o método.</span><span class="sxs-lookup"><span data-stu-id="8494a-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="8494a-154">Em seguida, o código salva os dados.</span><span class="sxs-lookup"><span data-stu-id="8494a-154">The code then saves the data.</span></span> <span data-ttu-id="8494a-155">Em seguida, a consulta LINQ que retorna um objeto de universidade onde seu local está mais próximo do local especificado é construída e executada.</span><span class="sxs-lookup"><span data-stu-id="8494a-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="8494a-156">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8494a-156">Compile and run the application.</span></span> <span data-ttu-id="8494a-157">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="8494a-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="8494a-158">Exibir o banco de dados gerado</span><span class="sxs-lookup"><span data-stu-id="8494a-158">View the Generated Database</span></span>

<span data-ttu-id="8494a-159">Quando você executa o aplicativo pela primeira vez, o Entity Framework cria um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="8494a-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="8494a-160">Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="8494a-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="8494a-161">Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (neste exemplo, é **SpatialCodeFirst. UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="8494a-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="8494a-162">As próximas vezes que o banco de dados existente será usado.</span><span class="sxs-lookup"><span data-stu-id="8494a-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="8494a-163">Observe que, se você fizer alterações em seu modelo depois que o banco de dados tiver sido criado, deverá usar Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8494a-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="8494a-164">Consulte [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar migrações.</span><span class="sxs-lookup"><span data-stu-id="8494a-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="8494a-165">Para exibir o banco de dados e os dados, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="8494a-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="8494a-166">No menu principal do Visual Studio 2012, selecione **exibir** -&gt; **pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8494a-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="8494a-167">Se o LocalDB não estiver na lista de servidores, clique com o botão direito do mouse em **SQL Server** e selecione **Adicionar SQL Server** usar a autenticação padrão do **Windows** para se conectar à instância de LocalDB</span><span class="sxs-lookup"><span data-stu-id="8494a-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="8494a-168">Expandir o nó LocalDB</span><span class="sxs-lookup"><span data-stu-id="8494a-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="8494a-169">Desdobrar a pasta **databases** para ver o novo banco de dados e navegar até a tabela **universidades**</span><span class="sxs-lookup"><span data-stu-id="8494a-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="8494a-170">Para exibir dados, clique com o botão direito do mouse na tabela e selecione **exibir dados**</span><span class="sxs-lookup"><span data-stu-id="8494a-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="8494a-171">Resumo</span><span class="sxs-lookup"><span data-stu-id="8494a-171">Summary</span></span>

<span data-ttu-id="8494a-172">Neste tutorial, vimos como usar tipos espaciais com Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="8494a-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
