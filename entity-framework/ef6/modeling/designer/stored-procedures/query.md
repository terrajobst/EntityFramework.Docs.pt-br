---
title: Designer de consulta armazenados procedimentos - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 6284b10261e6f3b9bf69d1c15e121988e4976d48
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489941"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="9a8dd-102">Designer de consulta procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="9a8dd-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="9a8dd-103">Este passo a passo mostram como usar o Entity Framework Designer (Designer de EF) para importar os procedimentos armazenados em um modelo e, em seguida, chamar procedimentos armazenados para recuperar resultados importados.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="9a8dd-104">Observe que o Code First não suporta o mapeamento para procedimentos armazenados ou funções.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="9a8dd-105">No entanto, você pode chamar procedimentos armazenados ou funções por meio do método System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="9a8dd-106">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9a8dd-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="9a8dd-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="9a8dd-107">Prerequisites</span></span>

<span data-ttu-id="9a8dd-108">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="9a8dd-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="9a8dd-109">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="9a8dd-110">O [banco de dados de exemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="9a8dd-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9a8dd-111">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="9a8dd-111">Set up the Project</span></span>

-   <span data-ttu-id="9a8dd-112">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="9a8dd-113">Selecione **arquivo -&gt; New -&gt; projeto**</span><span class="sxs-lookup"><span data-stu-id="9a8dd-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="9a8dd-114">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="9a8dd-115">Insira **EFwithSProcsSample** como o nome.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="9a8dd-116">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="9a8dd-117">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="9a8dd-117">Create a Model</span></span>

-   <span data-ttu-id="9a8dd-118">Clique com botão direito no projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="9a8dd-119">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="9a8dd-120">Insira **EFwithSProcsModel.edmx** para o nome de arquivo e clique **Add**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="9a8dd-121">Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="9a8dd-122">Clique em **nova Conexão**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="9a8dd-123">Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="9a8dd-124">A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="9a8dd-125">Na caixa de diálogo Choose Your Database Objects, verifique as **tabelas** caixa de seleção para selecionar todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="9a8dd-126">Além disso, selecione os seguintes procedimentos armazenados sob a **procedimentos armazenados e funções** nó: **GetStudentGrades** e **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Importar](~/ef6/media/import.jpg)

    <span data-ttu-id="9a8dd-128">*Começando com o Visual Studio 2012, o EF Designer oferece suporte a importação em massa de procedimentos armazenados. O **importação selecionado procedimentos armazenados e funções no modelo theentity** é marcada por padrão.*</span><span class="sxs-lookup"><span data-stu-id="9a8dd-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="9a8dd-129">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-129">Click **Finish**.</span></span>

<span data-ttu-id="9a8dd-130">Por padrão, a forma de resultado de cada importados de procedimento armazenado ou função que retorna mais de uma coluna tornará automaticamente um novo tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="9a8dd-131">Neste exemplo desejamos mapear os resultados do **GetStudentGrades** funcionar para o **StudentGrade** entidade e os resultados do **GetDepartmentName** para **none** (**none** é o valor padrão).</span><span class="sxs-lookup"><span data-stu-id="9a8dd-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="9a8dd-132">Para uma importação de função retornar um tipo de entidade, as colunas retornadas pelo procedimento armazenado correspondente devem corresponder exatamente as propriedades escalares do tipo de entidade retornada.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="9a8dd-133">Uma importação de função também pode retornar coleções de tipos simples, tipos complexos ou nenhum valor.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="9a8dd-134">Clique com botão direito a superfície de design e selecione **Model Browser**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="9a8dd-135">Na **Model Browser**, selecione **Function Imports**e, em seguida, clique duas vezes o **GetStudentGrades** função.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="9a8dd-136">Na caixa de diálogo Editar importação de função, selecione **entidades** e escolha **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="9a8dd-137">*O **importação de função é passível de composição** na parte superior da caixa de seleção a **Function Imports** diálogo permitirá que você mapear para funções combináveis. Se você marcar esta caixa, apenas funções combináveis (funções com valor de tabela) aparecerá na **procedimento armazenado / nome da função** lista suspensa. Se você não marcar essa caixa, apenas as funções não combináveis serão mostradas na lista.*</span><span class="sxs-lookup"><span data-stu-id="9a8dd-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="9a8dd-138">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="9a8dd-138">Use the Model</span></span>

<span data-ttu-id="9a8dd-139">Abra o **Program.cs** do arquivo onde o **Main** método é definido.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="9a8dd-140">Adicione o seguinte código na função principal.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="9a8dd-141">O código chama dois procedimentos armazenados: **GetStudentGrades** (retorna **StudentGrades** especificado *StudentId*) e **GetDepartmentName** (retorna o nome do departamento no parâmetro de saída).</span><span class="sxs-lookup"><span data-stu-id="9a8dd-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="9a8dd-142">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-142">Compile and run the application.</span></span> <span data-ttu-id="9a8dd-143">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="9a8dd-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="9a8dd-144">Parâmetros de saída</span><span class="sxs-lookup"><span data-stu-id="9a8dd-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="9a8dd-145">Se forem usados parâmetros de saída, seus valores não estará disponíveis até que os resultados foram lidas completamente.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="9a8dd-146">Isso é devido ao comportamento subjacente do DbDataReader, consulte [recuperando dados usando um DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="9a8dd-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
