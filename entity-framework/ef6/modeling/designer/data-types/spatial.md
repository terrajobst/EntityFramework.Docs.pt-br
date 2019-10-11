---
title: Spatial-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182510"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="ed47b-102">Espacial-designer de EF</span><span class="sxs-lookup"><span data-stu-id="ed47b-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="ed47b-103">**Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="ed47b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="ed47b-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="ed47b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ed47b-105">O vídeo e instruções passo a passo mostram como mapear tipos espaciais com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="ed47b-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="ed47b-106">Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.</span><span class="sxs-lookup"><span data-stu-id="ed47b-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="ed47b-107">Este passo a passos usará Model First para criar um novo banco de dados, mas o designer do EF também pode ser usado com o fluxo de trabalho de [Database First](~/ef6/modeling/designer/workflows/database-first.md) para mapear para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="ed47b-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="ed47b-108">O suporte ao tipo espacial foi introduzido no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="ed47b-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="ed47b-109">Observe que para usar os novos recursos, como tipo espacial, enums e funções com valor de tabela, você deve direcionar .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="ed47b-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="ed47b-110">O Visual Studio 2012 visa o .NET 4,5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="ed47b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="ed47b-111">Para usar os tipos de dados espaciais, você também deve usar um provedor de Entity Framework que tenha suporte espacial.</span><span class="sxs-lookup"><span data-stu-id="ed47b-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="ed47b-112">Consulte [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ed47b-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="ed47b-113">Há dois tipos de dados espaciais principais: Geografia e Geometry.</span><span class="sxs-lookup"><span data-stu-id="ed47b-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="ed47b-114">O tipo de dados geography armazena dados dados elipsoidais (por exemplo, coordenadas de latitude e longitude de GPS).</span><span class="sxs-lookup"><span data-stu-id="ed47b-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="ed47b-115">O tipo de dados geometry representa o sistema de coordenadas euclidiana (simples).</span><span class="sxs-lookup"><span data-stu-id="ed47b-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="ed47b-116">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="ed47b-116">Watch the video</span></span>
<span data-ttu-id="ed47b-117">Este vídeo mostra como mapear tipos espaciais com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="ed47b-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="ed47b-118">Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.</span><span class="sxs-lookup"><span data-stu-id="ed47b-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="ed47b-119">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="ed47b-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="ed47b-120">**Vídeo**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="ed47b-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ed47b-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ed47b-121">Pre-Requisites</span></span>

<span data-ttu-id="ed47b-122">Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.</span><span class="sxs-lookup"><span data-stu-id="ed47b-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ed47b-123">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="ed47b-123">Set up the Project</span></span>

1.  <span data-ttu-id="ed47b-124">Abrir o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ed47b-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="ed47b-125">No menu **arquivo** , aponte para **novo**e clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="ed47b-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="ed47b-126">No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console**</span><span class="sxs-lookup"><span data-stu-id="ed47b-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="ed47b-127">Insira **SpatialEFDesigner** como o nome do projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="ed47b-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="ed47b-128">Criar um novo modelo usando o designer do EF</span><span class="sxs-lookup"><span data-stu-id="ed47b-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="ed47b-129">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**</span><span class="sxs-lookup"><span data-stu-id="ed47b-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="ed47b-130">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos</span><span class="sxs-lookup"><span data-stu-id="ed47b-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="ed47b-131">Digite **UniversityModel. edmx** para o nome do arquivo e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="ed47b-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="ed47b-132">Na página Assistente de Modelo de Dados de Entidade, selecione **modelo vazio** na caixa de diálogo escolher conteúdo do modelo</span><span class="sxs-lookup"><span data-stu-id="ed47b-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="ed47b-133">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="ed47b-133">Click **Finish**</span></span>

<span data-ttu-id="ed47b-134">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="ed47b-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="ed47b-135">O assistente executa as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="ed47b-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="ed47b-136">Gera o arquivo EnumTestModel. edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles.</span><span class="sxs-lookup"><span data-stu-id="ed47b-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="ed47b-137">Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no assembly de saída para que os arquivos de metadados gerados sejam inseridos no assembly.</span><span class="sxs-lookup"><span data-stu-id="ed47b-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="ed47b-138">Adiciona uma referência aos seguintes assemblies: EntityFramework, System. ComponentModel. Annotations e System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="ed47b-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="ed47b-139">Cria arquivos UniversityModel.tt e UniversityModel.Context.tt e os adiciona no arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="ed47b-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="ed47b-140">Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e os tipos POCO que são mapeados para as entidades no modelo. edmx</span><span class="sxs-lookup"><span data-stu-id="ed47b-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="ed47b-141">Adicionar um novo tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="ed47b-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="ed47b-142">Clique com o botão direito do mouse em uma área vazia da superfície de design, selecione **entidade Add-&gt;** , a caixa de diálogo nova entidade será exibida</span><span class="sxs-lookup"><span data-stu-id="ed47b-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="ed47b-143">Especifique **University** para o nome do tipo e especifique **universityid** para o nome da propriedade de chave, deixe o tipo como **Int32**</span><span class="sxs-lookup"><span data-stu-id="ed47b-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="ed47b-144">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="ed47b-144">Click **OK**</span></span>
4.  <span data-ttu-id="ed47b-145">Clique com o botão direito do mouse na entidade e selecione **Adicionar New-&gt; Propriedade escalar**</span><span class="sxs-lookup"><span data-stu-id="ed47b-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="ed47b-146">Renomear a nova propriedade como **nome**</span><span class="sxs-lookup"><span data-stu-id="ed47b-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="ed47b-147">Adicione outra propriedade escalar e renomeie-a para o **local** abra o janela Propriedades e altere o tipo da nova propriedade para **geografia**</span><span class="sxs-lookup"><span data-stu-id="ed47b-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="ed47b-148">Salvar o modelo e compilar o projeto</span><span class="sxs-lookup"><span data-stu-id="ed47b-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="ed47b-149">Quando você cria, avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros.</span><span class="sxs-lookup"><span data-stu-id="ed47b-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="ed47b-150">Você pode ignorar esses avisos porque, depois de escolhermos para gerar o banco de dados do modelo, os erros serão descontinuados.</span><span class="sxs-lookup"><span data-stu-id="ed47b-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="ed47b-151">Gerar banco de dados do modelo</span><span class="sxs-lookup"><span data-stu-id="ed47b-151">Generate Database from Model</span></span>

<span data-ttu-id="ed47b-152">Agora, podemos gerar um banco de dados baseado no modelo.</span><span class="sxs-lookup"><span data-stu-id="ed47b-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="ed47b-153">Clique com o botão direito do mouse em um espaço vazio na superfície de Entity Designer e selecione **gerar banco de dados do modelo**</span><span class="sxs-lookup"><span data-stu-id="ed47b-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="ed47b-154">A caixa de diálogo escolher sua conexão de dados do assistente para gerar banco de dados é exibida clique no botão **nova conexão** especificar **(LocalDB) \\mssqllocaldb** para o nome do servidor e a **Universidade** para o banco de dados e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="ed47b-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="ed47b-155">Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="ed47b-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="ed47b-156">Clique em **Avançar** e o assistente para criar banco de dados gera DDL (linguagem de definição de dado) para criar um banco de dados o DDL gerado é exibido na caixa de diálogo Resumo e configurações Observe que o DDL não contém uma definição para uma tabela que mapeia para o tipo de enumeração</span><span class="sxs-lookup"><span data-stu-id="ed47b-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="ed47b-157">Clique em **concluir** clicando em concluir não executa o script DDL.</span><span class="sxs-lookup"><span data-stu-id="ed47b-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="ed47b-158">O assistente para criar banco de dados faz o seguinte: Abre o **UniversityModel. edmx. SQL** no Editor T-SQL gera as seções esquema de armazenamento e mapeamento do arquivo EDMX adiciona informações de cadeia de conexão ao arquivo app. config</span><span class="sxs-lookup"><span data-stu-id="ed47b-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="ed47b-159">Clique com o botão direito do mouse no Editor T-SQL e selecione **executar** a caixa de diálogo conectar ao servidor aparece, insira as informações de conexão da etapa 2 e clique em **conectar**</span><span class="sxs-lookup"><span data-stu-id="ed47b-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="ed47b-160">Para exibir o esquema gerado, clique com o botão direito do mouse no nome do banco de dados em Pesquisador de Objetos do SQL Server e selecione **Atualizar**</span><span class="sxs-lookup"><span data-stu-id="ed47b-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="ed47b-161">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="ed47b-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="ed47b-162">Abra o arquivo Program.cs em que o método Main está definido.</span><span class="sxs-lookup"><span data-stu-id="ed47b-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="ed47b-163">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="ed47b-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="ed47b-164">O código adiciona dois novos objetos University ao contexto.</span><span class="sxs-lookup"><span data-stu-id="ed47b-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="ed47b-165">As propriedades espaciais são inicializadas usando o método DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="ed47b-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="ed47b-166">O ponto de Geografia representado como WellKnownText é passado para o método.</span><span class="sxs-lookup"><span data-stu-id="ed47b-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="ed47b-167">Em seguida, o código salva os dados.</span><span class="sxs-lookup"><span data-stu-id="ed47b-167">The code then saves the data.</span></span> <span data-ttu-id="ed47b-168">Em seguida, a consulta LINQ que retorna um objeto de universidade onde seu local está mais próximo do local especificado é construída e executada.</span><span class="sxs-lookup"><span data-stu-id="ed47b-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

<span data-ttu-id="ed47b-169">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed47b-169">Compile and run the application.</span></span> <span data-ttu-id="ed47b-170">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="ed47b-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="ed47b-171">Para exibir dados no banco de dados, clique com o botão direito do mouse no nome do banco de dado em Pesquisador de Objetos do SQL Server e selecione **Atualizar**.</span><span class="sxs-lookup"><span data-stu-id="ed47b-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="ed47b-172">Em seguida, clique com o botão direito do mouse na tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="ed47b-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="ed47b-173">Resumo</span><span class="sxs-lookup"><span data-stu-id="ed47b-173">Summary</span></span>

<span data-ttu-id="ed47b-174">Neste tutorial, vimos como mapear tipos espaciais usando o Entity Framework Designer e como usar tipos espaciais no código.</span><span class="sxs-lookup"><span data-stu-id="ed47b-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
