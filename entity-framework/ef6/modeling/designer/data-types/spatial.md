---
title: Espaciais - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 39fe54ffe9d0ffd1753844f7bffcd1832d82eec5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994361"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="771d2-102">Espaciais - EF Designer</span><span class="sxs-lookup"><span data-stu-id="771d2-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="771d2-103">**EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="771d2-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="771d2-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="771d2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="771d2-105">O passo a passo e vídeo passo a passo mostra como mapear tipos espaciais com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="771d2-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="771d2-106">Ele também demonstra como usar uma consulta LINQ para localizar uma distância entre dois locais.</span><span class="sxs-lookup"><span data-stu-id="771d2-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="771d2-107">Este passo a passo usará Model First para criar um novo banco de dados, mas o EF Designer também pode ser usado com o [Database First](~/ef6/modeling/designer/workflows/database-first.md) fluxo de trabalho para mapear para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="771d2-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="771d2-108">Suporte a tipo espacial foi introduzido no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="771d2-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="771d2-109">Observe que, para usar os novos recursos, como o tipo espacial, enumerações e funções com valor de tabela, você deve direcionar o .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="771d2-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="771d2-110">Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="771d2-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="771d2-111">Para usar tipos de dados espaciais, você também deve usar um provedor do Entity Framework que tenha suporte espacial.</span><span class="sxs-lookup"><span data-stu-id="771d2-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="771d2-112">Ver [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="771d2-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="771d2-113">Há dois tipos de dados espaciais principal: geography e geometry.</span><span class="sxs-lookup"><span data-stu-id="771d2-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="771d2-114">O tipo de dados geography armazena dados elipsoidais (por exemplo, GPS latitude e longitude coordenadas).</span><span class="sxs-lookup"><span data-stu-id="771d2-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="771d2-115">O tipo de dados geometry representa o sistema de coordenadas Euclidiano (plano).</span><span class="sxs-lookup"><span data-stu-id="771d2-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="771d2-116">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="771d2-116">Watch the video</span></span>
<span data-ttu-id="771d2-117">Este vídeo mostra como mapear tipos espaciais com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="771d2-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="771d2-118">Ele também demonstra como usar uma consulta LINQ para localizar uma distância entre dois locais.</span><span class="sxs-lookup"><span data-stu-id="771d2-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="771d2-119">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="771d2-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="771d2-120">**Vídeo**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="771d2-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="771d2-121">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="771d2-121">Pre-Requisites</span></span>

<span data-ttu-id="771d2-122">Você precisará ter o Visual Studio 2012 Ultimate, Premium, Professional ou Web Express edition instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="771d2-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="771d2-123">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="771d2-123">Set up the Project</span></span>

1.  <span data-ttu-id="771d2-124">Abra o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="771d2-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="771d2-125">Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="771d2-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="771d2-126">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo</span><span class="sxs-lookup"><span data-stu-id="771d2-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="771d2-127">Insira **SpatialEFDesigner** como o nome do projeto e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="771d2-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="771d2-128">Criar um novo modelo usando o Designer de EF</span><span class="sxs-lookup"><span data-stu-id="771d2-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="771d2-129">Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**</span><span class="sxs-lookup"><span data-stu-id="771d2-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="771d2-130">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos</span><span class="sxs-lookup"><span data-stu-id="771d2-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="771d2-131">Insira **UniversityModel.edmx** para o nome de arquivo e clique **adicionar**</span><span class="sxs-lookup"><span data-stu-id="771d2-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="771d2-132">Na página do Assistente de modelo de dados de entidade, selecione **modelo vazio** na caixa de diálogo Escolher conteúdo do modelo</span><span class="sxs-lookup"><span data-stu-id="771d2-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="771d2-133">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="771d2-133">Click **Finish**</span></span>

<span data-ttu-id="771d2-134">O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="771d2-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="771d2-135">O assistente executa as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="771d2-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="771d2-136">Gera o arquivo EnumTestModel.edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles.</span><span class="sxs-lookup"><span data-stu-id="771d2-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="771d2-137">Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no Assembly de saída para que os arquivos de metadados gerados obterem inseridos no assembly.</span><span class="sxs-lookup"><span data-stu-id="771d2-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="771d2-138">Adiciona uma referência aos assemblies a seguir: EntityFramework, DataAnnotations e Entity.</span><span class="sxs-lookup"><span data-stu-id="771d2-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="771d2-139">Cria arquivos UniversityModel.tt e UniversityModel.Context.tt e adiciona-o sob o arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="771d2-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="771d2-140">Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e tipos POCO que mapeiam para as entidades no modelo. edmx</span><span class="sxs-lookup"><span data-stu-id="771d2-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="771d2-141">Adicionar um novo tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="771d2-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="771d2-142">Com uma área vazia da superfície de design, selecione o botão direito **Add -&gt; entidade**, será exibida a caixa de diálogo nova entidade</span><span class="sxs-lookup"><span data-stu-id="771d2-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="771d2-143">Especificar **University** para o tipo de nome e especifique **UniversityID** para o nome de propriedade de chave, deixe o tipo como **Int32**</span><span class="sxs-lookup"><span data-stu-id="771d2-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="771d2-144">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="771d2-144">Click **OK**</span></span>
4.  <span data-ttu-id="771d2-145">A entidade com o botão direito e selecione **adicionar novo -&gt; propriedade escalar**</span><span class="sxs-lookup"><span data-stu-id="771d2-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="771d2-146">Renomeie a nova propriedade **nome**</span><span class="sxs-lookup"><span data-stu-id="771d2-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="771d2-147">Adicionar outra propriedade escalar e renomeie-o para **local** abra a janela Propriedades e altere o tipo da nova propriedade para **geografia**</span><span class="sxs-lookup"><span data-stu-id="771d2-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="771d2-148">Salvar o modelo e compilar o projeto</span><span class="sxs-lookup"><span data-stu-id="771d2-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="771d2-149">Quando você compila, avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros.</span><span class="sxs-lookup"><span data-stu-id="771d2-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="771d2-150">Você pode ignorar esses avisos porque depois que podemos optar por gerar o banco de dados do modelo, os erros serão eliminados.</span><span class="sxs-lookup"><span data-stu-id="771d2-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="771d2-151">Gerar o banco de dados de modelo</span><span class="sxs-lookup"><span data-stu-id="771d2-151">Generate Database from Model</span></span>

<span data-ttu-id="771d2-152">Agora podemos gerar um banco de dados com base no modelo.</span><span class="sxs-lookup"><span data-stu-id="771d2-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="771d2-153">Clique com botão direito no Entity Designer na superfície e selecione uma área vazia **gerar banco de dados de modelo**</span><span class="sxs-lookup"><span data-stu-id="771d2-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="771d2-154">Escolha sua caixa de diálogo de Conexão de dados do Assistente para gerar banco de dados é exibido clique a **nova Conexão** botão especificar **(localdb)\\mssqllocaldb** para o nome do servidor e o  **Universidade** para o banco de dados e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="771d2-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="771d2-155">Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="771d2-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="771d2-156">Clique em **próxima** e o Assistente para criar banco de dados gera linguagem de definição de dados (DDL) para criar um banco de dados DDL gerado é exibido no resumo e as configurações de caixa de diálogo caixa de anotação, que o DDL não contém uma definição para um tabela que mapeia para o tipo de enumeração</span><span class="sxs-lookup"><span data-stu-id="771d2-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="771d2-157">Clique em **concluir** clicar em Concluir não executa o script DDL.</span><span class="sxs-lookup"><span data-stu-id="771d2-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="771d2-158">O Assistente para criar banco de dados faz o seguinte: abre a **UniversityModel.edmx.sql** no Editor T-SQL gera as seções de esquema e mapeamento de armazenamento do EDMX arquivo adiciona informações de cadeia de caracteres de conexão no arquivo App. config</span><span class="sxs-lookup"><span data-stu-id="771d2-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="771d2-159">Clique o botão direito do mouse no Editor T-SQL e selecione **Execute** conectar-se a caixa de diálogo do servidor for exibida, insira as informações de conexão da etapa 2 e clique em **Connect**</span><span class="sxs-lookup"><span data-stu-id="771d2-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="771d2-160">Para exibir o esquema gerado, clique com botão direito no nome do banco de dados no Pesquisador de objetos do SQL Server e selecione **atualizar**</span><span class="sxs-lookup"><span data-stu-id="771d2-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="771d2-161">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="771d2-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="771d2-162">Abra o arquivo Program.cs em que o método Main é definido.</span><span class="sxs-lookup"><span data-stu-id="771d2-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="771d2-163">Adicione o seguinte código na função principal.</span><span class="sxs-lookup"><span data-stu-id="771d2-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="771d2-164">O código adiciona dois novos objetos University para o contexto.</span><span class="sxs-lookup"><span data-stu-id="771d2-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="771d2-165">Propriedades espaciais são inicializadas por meio do método DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="771d2-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="771d2-166">O ponto de Geografia representado como WellKnownText é passado para o método.</span><span class="sxs-lookup"><span data-stu-id="771d2-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="771d2-167">O código, em seguida, salva os dados.</span><span class="sxs-lookup"><span data-stu-id="771d2-167">The code then saves the data.</span></span> <span data-ttu-id="771d2-168">Em seguida, a consulta LINQ que que retorna um objeto University onde é mais próxima do local especificado, a sua localização é construído e executado.</span><span class="sxs-lookup"><span data-stu-id="771d2-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="771d2-169">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="771d2-169">Compile and run the application.</span></span> <span data-ttu-id="771d2-170">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="771d2-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="771d2-171">Para exibir dados no banco de dados, clique com botão direito no nome do banco de dados no Pesquisador de objetos do SQL Server e selecione **Refresh**.</span><span class="sxs-lookup"><span data-stu-id="771d2-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="771d2-172">Em seguida, clique o botão direito do mouse na tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="771d2-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="771d2-173">Resumo</span><span class="sxs-lookup"><span data-stu-id="771d2-173">Summary</span></span>

<span data-ttu-id="771d2-174">Neste passo a passo, nós examinamos como mapear tipos espaciais usando o Entity Framework Designer e como usar tipos espaciais no código.</span><span class="sxs-lookup"><span data-stu-id="771d2-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
