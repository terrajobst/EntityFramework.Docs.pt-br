---
title: Suporte a enum - Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283714"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="e366e-102">Suporte a enum - Code First</span><span class="sxs-lookup"><span data-stu-id="e366e-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="e366e-103">**EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="e366e-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="e366e-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="e366e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e366e-105">Este passo a passo e vídeo passo a passo mostra como usar tipos de enumeração com o Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e366e-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="e366e-106">Ele também demonstra como usar enums em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="e366e-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="e366e-107">Este passo a passo usar Code First para criar um novo banco de dados, mas você também pode usar [Code First para mapear para um banco de dados](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="e366e-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="e366e-108">Suporte a enum foi introduzido no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="e366e-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="e366e-109">Para usar os novos recursos, como enums, tipos de dados espaciais e funções com valor de tabela, você deve ter como destino .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="e366e-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="e366e-110">Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="e366e-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="e366e-111">No Entity Framework, uma enumeração pode ter os seguintes tipos de base: **bytes**, **Int16**, **Int32**, **Int64** , ou **SByte**.</span><span class="sxs-lookup"><span data-stu-id="e366e-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e366e-112">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="e366e-112">Watch the video</span></span>
<span data-ttu-id="e366e-113">Este vídeo mostra como usar tipos de enumeração com o Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e366e-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="e366e-114">Ele também demonstra como usar enums em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="e366e-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="e366e-115">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="e366e-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="e366e-116">**Vídeo**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="e366e-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e366e-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e366e-117">Pre-Requisites</span></span>

<span data-ttu-id="e366e-118">Você precisará ter o Visual Studio 2012 Ultimate, Premium, Professional ou Web Express edition instalado para concluir este passo a passo.</span><span class="sxs-lookup"><span data-stu-id="e366e-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="e366e-119">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="e366e-119">Set up the Project</span></span>

1.  <span data-ttu-id="e366e-120">Abra o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e366e-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="e366e-121">Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="e366e-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="e366e-122">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo</span><span class="sxs-lookup"><span data-stu-id="e366e-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="e366e-123">Insira **EnumCodeFirst** como o nome do projeto e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="e366e-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="e366e-124">Definir um novo modelo usando o Code First</span><span class="sxs-lookup"><span data-stu-id="e366e-124">Define a New Model using Code First</span></span>

<span data-ttu-id="e366e-125">Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio).</span><span class="sxs-lookup"><span data-stu-id="e366e-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="e366e-126">O código a seguir define a classe de departamento.</span><span class="sxs-lookup"><span data-stu-id="e366e-126">The code below defines the Department class.</span></span>

<span data-ttu-id="e366e-127">O código também define a enumeração DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="e366e-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="e366e-128">Por padrão, a enumeração é de **int** tipo.</span><span class="sxs-lookup"><span data-stu-id="e366e-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="e366e-129">A propriedade de nome na classe departamento é do tipo DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="e366e-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="e366e-130">Abra o arquivo Program.cs e cole as seguintes definições de classe.</span><span class="sxs-lookup"><span data-stu-id="e366e-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="e366e-131">Definir tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="e366e-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="e366e-132">Além de definir entidades, você precisa definir uma classe que deriva de DbContext e expõe um DbSet&lt;TEntity&gt; propriedades.</span><span class="sxs-lookup"><span data-stu-id="e366e-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="e366e-133">O DbSet&lt;TEntity&gt; propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="e366e-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="e366e-134">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e366e-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="e366e-135">Os tipos DbContext e DbSet são definidos no assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="e366e-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="e366e-136">Adicionaremos uma referência a essa DLL usando o pacote EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="e366e-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="e366e-137">No Gerenciador de soluções, clique com botão direito no nome do projeto.</span><span class="sxs-lookup"><span data-stu-id="e366e-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="e366e-138">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="e366e-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="e366e-139">Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote.</span><span class="sxs-lookup"><span data-stu-id="e366e-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="e366e-140">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="e366e-140">Click **Install**</span></span>

<span data-ttu-id="e366e-141">Observe que, além do assembly EntityFramework, as referências aos assemblies System e de DataAnnotations são adicionadas também.</span><span class="sxs-lookup"><span data-stu-id="e366e-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="e366e-142">Na parte superior do arquivo Program.cs, adicione a seguinte instrução using:</span><span class="sxs-lookup"><span data-stu-id="e366e-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="e366e-143">De Program.cs, adicione a definição de contexto.</span><span class="sxs-lookup"><span data-stu-id="e366e-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="e366e-144">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="e366e-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="e366e-145">Abra o arquivo Program.cs em que o método Main é definido.</span><span class="sxs-lookup"><span data-stu-id="e366e-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="e366e-146">Adicione o seguinte código na função principal.</span><span class="sxs-lookup"><span data-stu-id="e366e-146">Add the following code into the Main function.</span></span> <span data-ttu-id="e366e-147">O código adiciona um novo objeto de departamento para o contexto.</span><span class="sxs-lookup"><span data-stu-id="e366e-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="e366e-148">Em seguida, ele salva os dados.</span><span class="sxs-lookup"><span data-stu-id="e366e-148">It then saves the data.</span></span> <span data-ttu-id="e366e-149">O código também executa uma consulta LINQ que retorna um departamento em que o nome é DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="e366e-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="e366e-150">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e366e-150">Compile and run the application.</span></span> <span data-ttu-id="e366e-151">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="e366e-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="e366e-152">Exibir o banco de dados gerado</span><span class="sxs-lookup"><span data-stu-id="e366e-152">View the Generated Database</span></span>

<span data-ttu-id="e366e-153">Quando você executa o aplicativo na primeira vez, o Entity Framework cria um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="e366e-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="e366e-154">Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância do LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e366e-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="e366e-155">Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (neste exemplo que é **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="e366e-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="e366e-156">As próximas vezes que o banco de dados existente será usado.</span><span class="sxs-lookup"><span data-stu-id="e366e-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="e366e-157">Observe que, se você fizer alterações ao seu modelo depois que o banco de dados for criado, você deve usar migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e366e-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="e366e-158">Ver [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar as migrações.</span><span class="sxs-lookup"><span data-stu-id="e366e-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="e366e-159">Para exibir o banco de dados e os dados, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e366e-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="e366e-160">No menu principal do Visual Studio 2012, selecione **modo de exibição**  - &gt; **Pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e366e-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="e366e-161">Se LocalDB não estiver na lista de servidores, clique o botão direito do mouse em **do SQL Server** e selecione **adicionar SQL Server** Use o padrão **autenticação do Windows** para conectar-se para o Instância de LocalDB</span><span class="sxs-lookup"><span data-stu-id="e366e-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="e366e-162">Expanda o nó do LocalDB</span><span class="sxs-lookup"><span data-stu-id="e366e-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="e366e-163">Desdobrar o **bancos de dados** pasta para ver o novo banco de dados e navegue até a **departamento** Observação, o que o Code First não cria uma tabela que mapeia para o tipo de enumeração de tabela</span><span class="sxs-lookup"><span data-stu-id="e366e-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="e366e-164">Para exibir os dados, clique com botão direito na tabela e selecione **exibir dados**</span><span class="sxs-lookup"><span data-stu-id="e366e-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="e366e-165">Resumo</span><span class="sxs-lookup"><span data-stu-id="e366e-165">Summary</span></span>

<span data-ttu-id="e366e-166">Neste passo a passo analisamos como usar tipos de enumeração com o Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e366e-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
