---
title: Funções com valor de tabela (TVFs) – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418693"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="9a631-102">Funções com valor de tabela (TVFs)</span><span class="sxs-lookup"><span data-stu-id="9a631-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="9a631-103">**Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="9a631-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="9a631-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="9a631-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9a631-105">O vídeo e instruções passo a passo mostram como mapear TVFs (funções com valor de tabela) usando o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9a631-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="9a631-106">Ele também demonstra como chamar um TVF de uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="9a631-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="9a631-107">Atualmente, os TVFs só têm suporte no fluxo de trabalho Database First.</span><span class="sxs-lookup"><span data-stu-id="9a631-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="9a631-108">O suporte a TVF foi introduzido na versão 5 do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9a631-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="9a631-109">Observe que para usar os novos recursos, como funções com valor de tabela, enums e tipos espaciais, você deve direcionar .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="9a631-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="9a631-110">O Visual Studio 2012 visa o .NET 4,5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="9a631-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="9a631-111">TVFs são muito semelhantes aos procedimentos armazenados com uma diferença importante: o resultado de um TVF é combinável.</span><span class="sxs-lookup"><span data-stu-id="9a631-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="9a631-112">Isso significa que os resultados de um TVF podem ser usados em uma consulta LINQ, enquanto os resultados de um procedimento armazenado não podem.</span><span class="sxs-lookup"><span data-stu-id="9a631-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="9a631-113">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="9a631-113">Watch the video</span></span>

<span data-ttu-id="9a631-114">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="9a631-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="9a631-115">[Wmv](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (zip)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="9a631-115">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="9a631-116">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="9a631-116">Pre-Requisites</span></span>

<span data-ttu-id="9a631-117">Para concluir este passo a passos, você precisa:</span><span class="sxs-lookup"><span data-stu-id="9a631-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="9a631-118">Instale o [banco de dados escolar](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="9a631-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="9a631-119">Ter uma versão recente do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9a631-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9a631-120">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="9a631-120">Set up the Project</span></span>

1.  <span data-ttu-id="9a631-121">Abra o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9a631-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="9a631-122">No menu **arquivo** , aponte para **novo**e clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="9a631-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="9a631-123">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console**</span><span class="sxs-lookup"><span data-stu-id="9a631-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="9a631-124">Insira **TVF** como o nome do projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="9a631-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="9a631-125">Adicionar um TVF ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="9a631-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="9a631-126">Selecione **Exibir-&gt; pesquisador de objetos do SQL Server**</span><span class="sxs-lookup"><span data-stu-id="9a631-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="9a631-127">Se o LocalDB não estiver na lista de servidores: clique com o botão direito do mouse em **SQL Server** e selecione **Adicionar SQL Server** usar a autenticação padrão do **Windows** para se conectar ao servidor LocalDB</span><span class="sxs-lookup"><span data-stu-id="9a631-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="9a631-128">Expandir o nó LocalDB</span><span class="sxs-lookup"><span data-stu-id="9a631-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="9a631-129">No nó bancos de dados, clique com o botão direito do mouse no nó escolar e selecione **nova consulta...**</span><span class="sxs-lookup"><span data-stu-id="9a631-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="9a631-130">No Editor T-SQL, Cole a seguinte definição de TVF</span><span class="sxs-lookup"><span data-stu-id="9a631-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="9a631-131">Clique com o botão direito do mouse no Editor T-SQL e selecione **executar**</span><span class="sxs-lookup"><span data-stu-id="9a631-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="9a631-132">A função GetStudentGradesForCourse é adicionada ao banco de dados School</span><span class="sxs-lookup"><span data-stu-id="9a631-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="9a631-133">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="9a631-133">Create a Model</span></span>

1.  <span data-ttu-id="9a631-134">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**</span><span class="sxs-lookup"><span data-stu-id="9a631-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="9a631-135">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel **modelos**</span><span class="sxs-lookup"><span data-stu-id="9a631-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="9a631-136">Digite **TVFModel. edmx** para o nome do arquivo e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="9a631-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="9a631-137">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="9a631-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="9a631-138">Clique em **nova conexão** inserir **(LocalDB)\\mssqllocaldb** na caixa de texto nome do servidor, digite **School** para o nome do banco de dados clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="9a631-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="9a631-139">Na caixa de diálogo escolher seu objeto de banco de dados, no nó **tabelas** , selecione a **pessoa**, **StudentGrade**e **curso** tabelas</span><span class="sxs-lookup"><span data-stu-id="9a631-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="9a631-140">Selecione a função **GetStudentGradesForCourse** localizada na observação **procedimentos armazenados e funções** nó Observe que, começando com o Visual Studio 2012, o Entity designer permite que você importe em lote seus procedimentos armazenados e funções</span><span class="sxs-lookup"><span data-stu-id="9a631-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="9a631-141">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="9a631-141">Click **Finish**</span></span>
9.  <span data-ttu-id="9a631-142">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="9a631-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="9a631-143">Todos os objetos que você selecionou na caixa de diálogo **escolher seus objetos de banco de dados** são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="9a631-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="9a631-144">Por padrão, a forma de resultado de cada procedimento armazenado ou função importada se tornará automaticamente um novo tipo complexo em seu modelo de entidade.</span><span class="sxs-lookup"><span data-stu-id="9a631-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="9a631-145">Mas queremos mapear os resultados da função GetStudentGradesForCourse para a entidade StudentGrade: clique com o botão direito do mouse na superfície de design e selecione **navegador de modelos** no navegador de modelos, selecione **importações de função**e clique duas vezes na função **GetStudentGradesForCourse** na caixa de diálogo Editar importação de função, selecione **entidades** e escolha **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="9a631-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="9a631-146">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="9a631-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="9a631-147">Abra o arquivo em que o método Main está definido.</span><span class="sxs-lookup"><span data-stu-id="9a631-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="9a631-148">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="9a631-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="9a631-149">O código a seguir demonstra como criar uma consulta que usa uma função com valor de tabela.</span><span class="sxs-lookup"><span data-stu-id="9a631-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="9a631-150">A consulta projeta os resultados em um tipo anônimo que contém o título do curso relacionado e os alunos relacionados com uma taxa maior ou igual a 3,5.</span><span class="sxs-lookup"><span data-stu-id="9a631-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="9a631-151">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9a631-151">Compile and run the application.</span></span> <span data-ttu-id="9a631-152">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="9a631-152">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="9a631-153">Resumo</span><span class="sxs-lookup"><span data-stu-id="9a631-153">Summary</span></span>

<span data-ttu-id="9a631-154">Neste tutorial, vimos como mapear TVFs (funções com valor de tabela) usando o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="9a631-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="9a631-155">Ele também demonstrou como chamar um TVF de uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="9a631-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
