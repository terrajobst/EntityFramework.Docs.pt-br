---
title: Funções com valor de tabela (TVFs) - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 6130e6bd550497509f3dcc0242077046d12c601a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489317"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="5468c-102">Funções com valor de tabela (TVFs)</span><span class="sxs-lookup"><span data-stu-id="5468c-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="5468c-103">**EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="5468c-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="5468c-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="5468c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="5468c-105">O passo a passo e vídeo passo a passo mostra como mapear funções avaliadas por tabela (TVFs) usando o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="5468c-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="5468c-106">Ele também demonstra como chamar uma TVF de uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="5468c-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="5468c-107">TVFs estão atualmente só tem suporte no banco de dados primeiro fluxo de trabalho.</span><span class="sxs-lookup"><span data-stu-id="5468c-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="5468c-108">Suporte TVF foi introduzido no Entity Framework versão 5.</span><span class="sxs-lookup"><span data-stu-id="5468c-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="5468c-109">Observe que, para usar os novos recursos, como funções com valor de tabela, enumerações e tipos espaciais, você deve ter como destino .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="5468c-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="5468c-110">Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.</span><span class="sxs-lookup"><span data-stu-id="5468c-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="5468c-111">TVFs são muito semelhantes aos procedimentos armazenados com uma diferença importante: o resultado de uma TVF é passível de composição.</span><span class="sxs-lookup"><span data-stu-id="5468c-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="5468c-112">Isso significa que os resultados de uma TVF podem ser usados em uma consulta LINQ, enquanto os resultados de um procedimento armazenado não é possível.</span><span class="sxs-lookup"><span data-stu-id="5468c-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="5468c-113">Assista ao vídeo</span><span class="sxs-lookup"><span data-stu-id="5468c-113">Watch the video</span></span>

<span data-ttu-id="5468c-114">**Apresentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="5468c-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="5468c-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="5468c-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="5468c-116">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5468c-116">Pre-Requisites</span></span>

<span data-ttu-id="5468c-117">Para concluir este passo a passo, você precisa:</span><span class="sxs-lookup"><span data-stu-id="5468c-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="5468c-118">Instalar o [banco de dados School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="5468c-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="5468c-119">Ter uma versão recente do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5468c-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5468c-120">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="5468c-120">Set up the Project</span></span>

1.  <span data-ttu-id="5468c-121">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5468c-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="5468c-122">Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**</span><span class="sxs-lookup"><span data-stu-id="5468c-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="5468c-123">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo</span><span class="sxs-lookup"><span data-stu-id="5468c-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="5468c-124">Insira **TVF** como o nome do projeto e clique em **Okey**</span><span class="sxs-lookup"><span data-stu-id="5468c-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="5468c-125">Adicionar uma TVF ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="5468c-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="5468c-126">Selecione **exibição -&gt; Pesquisador de objetos do SQL Server**</span><span class="sxs-lookup"><span data-stu-id="5468c-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="5468c-127">Se o LocalDB não está na lista de servidores: clique duas vezes em **do SQL Server** e selecione **adicionar SQL Server** Use o padrão **autenticação do Windows** para se conectar ao servidor do LocalDB</span><span class="sxs-lookup"><span data-stu-id="5468c-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="5468c-128">Expanda o nó do LocalDB</span><span class="sxs-lookup"><span data-stu-id="5468c-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="5468c-129">Sob o nó bancos de dados, clique com botão direito no nó do banco de dados de escola e selecione **nova consulta...**</span><span class="sxs-lookup"><span data-stu-id="5468c-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="5468c-130">No Editor T-SQL, cole a seguinte definição de TVF</span><span class="sxs-lookup"><span data-stu-id="5468c-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="5468c-131">Clique no botão direito do mouse no editor T-SQL e selecione **Execute**</span><span class="sxs-lookup"><span data-stu-id="5468c-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="5468c-132">A função GetStudentGradesForCourse é adicionada ao banco de dados School</span><span class="sxs-lookup"><span data-stu-id="5468c-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="5468c-133">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="5468c-133">Create a Model</span></span>

1.  <span data-ttu-id="5468c-134">Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**</span><span class="sxs-lookup"><span data-stu-id="5468c-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="5468c-135">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no **modelos** painel</span><span class="sxs-lookup"><span data-stu-id="5468c-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="5468c-136">Insira **TVFModel.edmx** para o nome de arquivo e clique **adicionar**</span><span class="sxs-lookup"><span data-stu-id="5468c-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="5468c-137">Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="5468c-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="5468c-138">Clique em **nova Conexão** Enter **(localdb)\\mssqllocaldb** no texto de nome de servidor caixa Enter **School** para o banco de dados nome clique **Okey**</span><span class="sxs-lookup"><span data-stu-id="5468c-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="5468c-139">Na escolha seu objetos de banco de dados caixa de diálogo do **tabelas** nó, selecione o **pessoa**, **StudentGrade**, e **curso** tabelas</span><span class="sxs-lookup"><span data-stu-id="5468c-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="5468c-140">Selecione o **GetStudentGradesForCourse** função localizado sob a **procedimentos armazenados e funções** nó nota, começando com o Visual Studio 2012, o Entity Designer permite a importação de lote seus procedimentos armazenados e funções</span><span class="sxs-lookup"><span data-stu-id="5468c-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="5468c-141">Clique em **concluir**</span><span class="sxs-lookup"><span data-stu-id="5468c-141">Click **Finish**</span></span>
9.  <span data-ttu-id="5468c-142">O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="5468c-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="5468c-143">Todos os objetos que você selecionou na **Choose Your Database Objects** caixa de diálogo são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="5468c-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="5468c-144">Por padrão, a forma de resultado de cada importados de procedimento armazenado ou função tornará automaticamente um novo tipo complexo em seu modelo de entidade.</span><span class="sxs-lookup"><span data-stu-id="5468c-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="5468c-145">Mas queremos mapear os resultados da função GetStudentGradesForCourse à entidade StudentGrade: a superfície de design e selecione com o botão direito **Model Browser** no Pesquisador de modelos, selecione **Function Imports**e, em seguida, clique duas vezes o **GetStudentGradesForCourse** caixa de diálogo no the Editar função importar função, selecione **entidades** e escolha **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="5468c-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="5468c-146">Persistir e recuperar dados</span><span class="sxs-lookup"><span data-stu-id="5468c-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="5468c-147">Abra o arquivo onde o método Main é definido.</span><span class="sxs-lookup"><span data-stu-id="5468c-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="5468c-148">Adicione o seguinte código na função principal.</span><span class="sxs-lookup"><span data-stu-id="5468c-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="5468c-149">O código a seguir demonstra como criar uma consulta que usa uma função com valor de tabela.</span><span class="sxs-lookup"><span data-stu-id="5468c-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="5468c-150">A consulta projeta os resultados em um tipo anônimo que contém o título do curso relacionado e os alunos relacionados com um nível maior ou igual a 3.5.</span><span class="sxs-lookup"><span data-stu-id="5468c-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="5468c-151">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5468c-151">Compile and run the application.</span></span> <span data-ttu-id="5468c-152">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="5468c-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="5468c-153">Resumo</span><span class="sxs-lookup"><span data-stu-id="5468c-153">Summary</span></span>

<span data-ttu-id="5468c-154">Neste passo a passo analisamos como mapear funções avaliadas por tabela (TVFs) usando o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="5468c-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="5468c-155">Ele também demonstrou como chamar uma TVF de uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="5468c-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
