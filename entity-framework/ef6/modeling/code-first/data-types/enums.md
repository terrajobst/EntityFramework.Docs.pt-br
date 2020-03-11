---
title: Suporte a enum-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419104"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="84c5e-102">Suporte a enum-Code First</span><span class="sxs-lookup"><span data-stu-id="84c5e-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="84c5e-103">**Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="84c5e-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="84c5e-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="84c5e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="84c5e-105">Este vídeo e instruções passo a passo mostram como usar tipos de enumeração com Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="84c5e-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="84c5e-106">Ele também demonstra como usar enums em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="84c5e-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="84c5e-107">Este passo a passos usará Code First para criar um novo banco de dados, mas você também pode usar [Code First para mapear para um banco de dados existente](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="84c5e-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="84c5e-108">O suporte a enum foi introduzido no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="84c5e-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="84c5e-109">Para usar os novos recursos como enums, tipos de dados espaciais e funções com valor de tabela, você deve direcionar .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="84c5e-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="84c5e-110">O Visual Studio 2012 visa o .NET 4,5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="84c5e-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="84c5e-111">No Entity Framework, uma enumeração pode ter os seguintes tipos subjacentes: **byte**, **Int16**, **Int32**, **Int64** ou **SByte**.</span><span class="sxs-lookup"><span data-stu-id="84c5e-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="84c5e-112">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="84c5e-112">Watch the video</span></span>
<span data-ttu-id="84c5e-113">Este vídeo mostra como usar tipos de enumeração com Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="84c5e-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="84c5e-114">Ele também demonstra como usar enums em uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="84c5e-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="84c5e-115">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="84c5e-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="84c5e-116">**Vídeo**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="84c5e-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="84c5e-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="84c5e-117">Pre-Requisites</span></span>

<span data-ttu-id="84c5e-118">Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.</span><span class="sxs-lookup"><span data-stu-id="84c5e-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="84c5e-119">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="84c5e-119">Set up the Project</span></span>

1.  <span data-ttu-id="84c5e-120">Abrir o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="84c5e-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="84c5e-121">No menu **arquivo** , aponte para **novo**e clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="84c5e-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="84c5e-122">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console**</span><span class="sxs-lookup"><span data-stu-id="84c5e-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="84c5e-123">Insira **EnumCodeFirst** como o nome do projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="84c5e-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="84c5e-124">Definir um novo modelo usando Code First</span><span class="sxs-lookup"><span data-stu-id="84c5e-124">Define a New Model using Code First</span></span>

<span data-ttu-id="84c5e-125">Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).</span><span class="sxs-lookup"><span data-stu-id="84c5e-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="84c5e-126">O código a seguir define a classe Department.</span><span class="sxs-lookup"><span data-stu-id="84c5e-126">The code below defines the Department class.</span></span>

<span data-ttu-id="84c5e-127">O código também define a enumeração Departmentnames.</span><span class="sxs-lookup"><span data-stu-id="84c5e-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="84c5e-128">Por padrão, a enumeração é do tipo **int** .</span><span class="sxs-lookup"><span data-stu-id="84c5e-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="84c5e-129">A propriedade Name na classe Department é do tipo Departmentnames.</span><span class="sxs-lookup"><span data-stu-id="84c5e-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="84c5e-130">Abra o arquivo Program.cs e cole as definições de classe a seguir.</span><span class="sxs-lookup"><span data-stu-id="84c5e-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="84c5e-131">Definir o tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="84c5e-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="84c5e-132">Além de definir entidades, você precisa definir uma classe derivada de DbContext e expõe DbSet&lt;TEntity&gt; Propriedades.</span><span class="sxs-lookup"><span data-stu-id="84c5e-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="84c5e-133">As propriedades DbSet&lt;TEntity&gt; permitem que o contexto saiba quais tipos você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="84c5e-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="84c5e-134">Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="84c5e-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="84c5e-135">Os tipos DbContext e DbSet são definidos no assembly do EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="84c5e-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="84c5e-136">Adicionaremos uma referência a essa DLL usando o pacote NuGet do EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="84c5e-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="84c5e-137">Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto.</span><span class="sxs-lookup"><span data-stu-id="84c5e-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="84c5e-138">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="84c5e-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="84c5e-139">Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="84c5e-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="84c5e-140">Clique em **Instalar**</span><span class="sxs-lookup"><span data-stu-id="84c5e-140">Click **Install**</span></span>

<span data-ttu-id="84c5e-141">Observe que, além do assembly do EntityFramework, as referências a assemblies System. ComponentModel. Annotations e System. Data. Entity também são adicionadas.</span><span class="sxs-lookup"><span data-stu-id="84c5e-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="84c5e-142">Na parte superior do arquivo Program.cs, adicione a seguinte instrução Using:</span><span class="sxs-lookup"><span data-stu-id="84c5e-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="84c5e-143">No Program.cs, adicione a definição de contexto.</span><span class="sxs-lookup"><span data-stu-id="84c5e-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="84c5e-144">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="84c5e-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="84c5e-145">Abra o arquivo Program.cs em que o método Main está definido.</span><span class="sxs-lookup"><span data-stu-id="84c5e-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="84c5e-146">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="84c5e-146">Add the following code into the Main function.</span></span> <span data-ttu-id="84c5e-147">O código adiciona um novo objeto Department ao contexto.</span><span class="sxs-lookup"><span data-stu-id="84c5e-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="84c5e-148">Em seguida, ele salva os dados.</span><span class="sxs-lookup"><span data-stu-id="84c5e-148">It then saves the data.</span></span> <span data-ttu-id="84c5e-149">O código também executa uma consulta LINQ que retorna um departamento onde o nome é Departmentnames. inglês.</span><span class="sxs-lookup"><span data-stu-id="84c5e-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="84c5e-150">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84c5e-150">Compile and run the application.</span></span> <span data-ttu-id="84c5e-151">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="84c5e-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="84c5e-152">Exibir o banco de dados gerado</span><span class="sxs-lookup"><span data-stu-id="84c5e-152">View the Generated Database</span></span>

<span data-ttu-id="84c5e-153">Quando você executa o aplicativo pela primeira vez, o Entity Framework cria um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="84c5e-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="84c5e-154">Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="84c5e-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="84c5e-155">Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (para este exemplo, **EnumCodeFirst. EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="84c5e-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="84c5e-156">As próximas vezes que o banco de dados existente será usado.</span><span class="sxs-lookup"><span data-stu-id="84c5e-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="84c5e-157">Observe que, se você fizer alterações em seu modelo depois que o banco de dados tiver sido criado, deverá usar Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="84c5e-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="84c5e-158">Consulte [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar migrações.</span><span class="sxs-lookup"><span data-stu-id="84c5e-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="84c5e-159">Para exibir o banco de dados e os dados, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="84c5e-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="84c5e-160">No menu principal do Visual Studio 2012, selecione **exibir** -&gt; **pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="84c5e-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="84c5e-161">Se o LocalDB não estiver na lista de servidores, clique com o botão direito do mouse em **SQL Server** e selecione **Adicionar SQL Server** usar a autenticação padrão do **Windows** para se conectar à instância do LocalDB</span><span class="sxs-lookup"><span data-stu-id="84c5e-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="84c5e-162">Expandir o nó LocalDB</span><span class="sxs-lookup"><span data-stu-id="84c5e-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="84c5e-163">Desdobrar a pasta **databases** para ver o novo banco de dados e navegar até a tabela **Department** Observe que Code First não cria uma tabela que é mapeada para o tipo de enumeração</span><span class="sxs-lookup"><span data-stu-id="84c5e-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="84c5e-164">Para exibir dados, clique com o botão direito do mouse na tabela e selecione **exibir dados**</span><span class="sxs-lookup"><span data-stu-id="84c5e-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="84c5e-165">Resumo</span><span class="sxs-lookup"><span data-stu-id="84c5e-165">Summary</span></span>

<span data-ttu-id="84c5e-166">Neste tutorial, vimos como usar tipos enum com Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="84c5e-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
