---
title: Suporte a enum-EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182514"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="41ba9-102">Suporte a enum-designer de EF</span><span class="sxs-lookup"><span data-stu-id="41ba9-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="41ba9-103">**Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="41ba9-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="41ba9-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="41ba9-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="41ba9-105">Este vídeo e instruções passo a passo mostram como usar tipos de enumeração com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="41ba9-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="41ba9-106">Ele também demonstra como usar enums em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="41ba9-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="41ba9-107">Este passo a passos usará Model First para criar um novo banco de dados, mas o designer do EF também pode ser usado com o fluxo de trabalho de [Database First](~/ef6/modeling/designer/workflows/database-first.md) para mapear para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="41ba9-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="41ba9-108">O suporte a enum foi introduzido no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="41ba9-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="41ba9-109">Para usar os novos recursos como enums, tipos de dados espaciais e funções com valor de tabela, você deve direcionar .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="41ba9-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="41ba9-110">O Visual Studio 2012 visa o .NET 4,5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="41ba9-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="41ba9-111">No Entity Framework, uma enumeração pode ter os seguintes tipos subjacentes: **Byte**, **Int16**, **Int32**, **Int64** ou **SByte**.</span><span class="sxs-lookup"><span data-stu-id="41ba9-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="41ba9-112">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="41ba9-112">Watch the Video</span></span>
<span data-ttu-id="41ba9-113">Este vídeo mostra como usar tipos de enumeração com o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="41ba9-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="41ba9-114">Ele também demonstra como usar enums em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="41ba9-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="41ba9-115">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="41ba9-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="41ba9-116">**Vídeo**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="41ba9-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="41ba9-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="41ba9-117">Pre-Requisites</span></span>

<span data-ttu-id="41ba9-118">Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.</span><span class="sxs-lookup"><span data-stu-id="41ba9-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="41ba9-119">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="41ba9-119">Set up the Project</span></span>

1.  <span data-ttu-id="41ba9-120">Abrir o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="41ba9-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="41ba9-121">No menu **arquivo** , aponte para **novo**e clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="41ba9-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="41ba9-122">No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console**</span><span class="sxs-lookup"><span data-stu-id="41ba9-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="41ba9-123">Insira **EnumEFDesigner** como o nome do projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="41ba9-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="41ba9-124">Criar um novo modelo usando o designer do EF</span><span class="sxs-lookup"><span data-stu-id="41ba9-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="41ba9-125">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**</span><span class="sxs-lookup"><span data-stu-id="41ba9-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="41ba9-126">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos</span><span class="sxs-lookup"><span data-stu-id="41ba9-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="41ba9-127">Digite **EnumTestModel. edmx** para o nome do arquivo e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="41ba9-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="41ba9-128">Na página Assistente de Modelo de Dados de Entidade, selecione **modelo vazio** na caixa de diálogo escolher conteúdo do modelo</span><span class="sxs-lookup"><span data-stu-id="41ba9-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="41ba9-129">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="41ba9-129">Click **Finish**</span></span>

<span data-ttu-id="41ba9-130">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="41ba9-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="41ba9-131">O assistente executa as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="41ba9-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="41ba9-132">Gera o arquivo EnumTestModel. edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles.</span><span class="sxs-lookup"><span data-stu-id="41ba9-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="41ba9-133">Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no assembly de saída para que os arquivos de metadados gerados sejam inseridos no assembly.</span><span class="sxs-lookup"><span data-stu-id="41ba9-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="41ba9-134">Adiciona uma referência aos seguintes assemblies: EntityFramework, System. ComponentModel. Annotations e System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="41ba9-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="41ba9-135">Cria arquivos EnumTestModel.tt e EnumTestModel.Context.tt e os adiciona no arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="41ba9-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="41ba9-136">Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e os tipos POCO que são mapeados para as entidades no modelo. edmx.</span><span class="sxs-lookup"><span data-stu-id="41ba9-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="41ba9-137">Adicionar um novo tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="41ba9-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="41ba9-138">Clique com o botão direito do mouse em uma área vazia da superfície de design, selecione **entidade Add-&gt;** , a caixa de diálogo nova entidade será exibida</span><span class="sxs-lookup"><span data-stu-id="41ba9-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="41ba9-139">Especifique **Department** para o nome do tipo e especifique **DepartmentID** para o nome da propriedade de chave, deixe o tipo como **Int32**</span><span class="sxs-lookup"><span data-stu-id="41ba9-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="41ba9-140">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="41ba9-140">Click **OK**</span></span>
4.  <span data-ttu-id="41ba9-141">Clique com o botão direito do mouse na entidade e selecione **Adicionar New-&gt; Propriedade escalar**</span><span class="sxs-lookup"><span data-stu-id="41ba9-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="41ba9-142">Renomear a nova propriedade como **nome**</span><span class="sxs-lookup"><span data-stu-id="41ba9-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="41ba9-143">Altere o tipo da nova propriedade para **Int32** (por padrão, a nova propriedade é do tipo cadeia de caracteres) para alterar o tipo, abra o janela Propriedades e altere a propriedade Type para **Int32**</span><span class="sxs-lookup"><span data-stu-id="41ba9-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="41ba9-144">Adicione outra propriedade escalar e renomeie-a para **orçamento**, altere o tipo para **decimal**</span><span class="sxs-lookup"><span data-stu-id="41ba9-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="41ba9-145">Adicionar um tipo de enumeração</span><span class="sxs-lookup"><span data-stu-id="41ba9-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="41ba9-146">Na Entity Framework Designer, clique com o botão direito do mouse na propriedade Name, selecione **converter em enum**</span><span class="sxs-lookup"><span data-stu-id="41ba9-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Converter em enumeração](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="41ba9-148">Na caixa de diálogo **Adicionar enum** , digite **departmentnames** para o nome do tipo de enumeração, altere o tipo subjacente para **Int32**e, em seguida, adicione os seguintes membros ao tipo: Inglês, matemática e economia</span><span class="sxs-lookup"><span data-stu-id="41ba9-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Adicionar tipo de enumeração](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="41ba9-150">Pressione **OK**</span><span class="sxs-lookup"><span data-stu-id="41ba9-150">Press **OK**</span></span>
4.  <span data-ttu-id="41ba9-151">Salvar o modelo e compilar o projeto</span><span class="sxs-lookup"><span data-stu-id="41ba9-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="41ba9-152">Quando você cria, avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros.</span><span class="sxs-lookup"><span data-stu-id="41ba9-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="41ba9-153">Você pode ignorar esses avisos porque, depois de escolhermos para gerar o banco de dados do modelo, os erros serão descontinuados.</span><span class="sxs-lookup"><span data-stu-id="41ba9-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="41ba9-154">Se você observar a janela Propriedades, observará que o tipo da propriedade Name foi alterado para **departmentnames** e o tipo de enumeração recém-adicionado foi adicionado à lista de tipos.</span><span class="sxs-lookup"><span data-stu-id="41ba9-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="41ba9-155">Se você alternar para a janela do navegador de modelos, verá que o tipo também foi adicionado ao nó tipos de enumeração.</span><span class="sxs-lookup"><span data-stu-id="41ba9-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Navegador de modelos](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="41ba9-157">Você também pode adicionar novos tipos de enumeração dessa janela clicando com o botão direito do mouse e selecionando **Adicionar tipo de enumeração**.</span><span class="sxs-lookup"><span data-stu-id="41ba9-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="41ba9-158">Depois que o tipo for criado, ele aparecerá na lista de tipos e você poderá associá-lo a uma propriedade</span><span class="sxs-lookup"><span data-stu-id="41ba9-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="41ba9-159">Gerar banco de dados do modelo</span><span class="sxs-lookup"><span data-stu-id="41ba9-159">Generate Database from Model</span></span>

<span data-ttu-id="41ba9-160">Agora, podemos gerar um banco de dados baseado no modelo.</span><span class="sxs-lookup"><span data-stu-id="41ba9-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="41ba9-161">Clique com o botão direito do mouse em um espaço vazio na superfície de Entity Designer e selecione **gerar banco de dados do modelo**</span><span class="sxs-lookup"><span data-stu-id="41ba9-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="41ba9-162">A caixa de diálogo escolher sua conexão de dados do assistente para gerar banco de dados é exibida clique no botão **nova conexão** especificar **(LocalDB) \\mssqllocaldb** para o nome do servidor e **EnumTest** para o banco de dados e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="41ba9-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="41ba9-163">Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="41ba9-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="41ba9-164">Clique em **Avançar** e o assistente para criar banco de dados gera DDL (linguagem de definição de dado) para criar um banco de dados o DDL gerado é exibido na caixa de diálogo Resumo e configurações Observe que o DDL não contém uma definição para uma tabela que mapeia para o tipo de enumeração</span><span class="sxs-lookup"><span data-stu-id="41ba9-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="41ba9-165">Clique em **concluir** clicando em concluir não executa o script DDL.</span><span class="sxs-lookup"><span data-stu-id="41ba9-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="41ba9-166">O assistente para criar banco de dados faz o seguinte: Abre o **EnumTest. edmx. SQL** no Editor T-SQL gera as seções esquema de armazenamento e mapeamento do arquivo EDMX adiciona informações de cadeia de conexão ao arquivo app. config</span><span class="sxs-lookup"><span data-stu-id="41ba9-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="41ba9-167">Clique com o botão direito do mouse no Editor T-SQL e selecione **executar** a caixa de diálogo conectar ao servidor aparece, insira as informações de conexão da etapa 2 e clique em **conectar**</span><span class="sxs-lookup"><span data-stu-id="41ba9-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="41ba9-168">Para exibir o esquema gerado, clique com o botão direito do mouse no nome do banco de dados em Pesquisador de Objetos do SQL Server e selecione **Atualizar**</span><span class="sxs-lookup"><span data-stu-id="41ba9-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="41ba9-169">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="41ba9-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="41ba9-170">Abra o arquivo Program.cs em que o método Main está definido.</span><span class="sxs-lookup"><span data-stu-id="41ba9-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="41ba9-171">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="41ba9-171">Add the following code into the Main function.</span></span> <span data-ttu-id="41ba9-172">O código adiciona um novo objeto Department ao contexto.</span><span class="sxs-lookup"><span data-stu-id="41ba9-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="41ba9-173">Em seguida, ele salva os dados.</span><span class="sxs-lookup"><span data-stu-id="41ba9-173">It then saves the data.</span></span> <span data-ttu-id="41ba9-174">O código também executa uma consulta LINQ que retorna um departamento onde o nome é Departmentnames. inglês.</span><span class="sxs-lookup"><span data-stu-id="41ba9-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="41ba9-175">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41ba9-175">Compile and run the application.</span></span> <span data-ttu-id="41ba9-176">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="41ba9-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="41ba9-177">Para exibir dados no banco de dados, clique com o botão direito do mouse no nome do banco de dado em Pesquisador de Objetos do SQL Server e selecione **Atualizar**.</span><span class="sxs-lookup"><span data-stu-id="41ba9-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="41ba9-178">Em seguida, clique com o botão direito do mouse na tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="41ba9-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="41ba9-179">Resumo</span><span class="sxs-lookup"><span data-stu-id="41ba9-179">Summary</span></span>

<span data-ttu-id="41ba9-180">Neste tutorial, vimos como mapear tipos de enumeração usando o Entity Framework Designer e como usar enums no código.</span><span class="sxs-lookup"><span data-stu-id="41ba9-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
