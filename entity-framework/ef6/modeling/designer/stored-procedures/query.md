---
title: Procedimentos armazenados de consulta do designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182479"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="800e2-102">Procedimentos armazenados de consulta do designer</span><span class="sxs-lookup"><span data-stu-id="800e2-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="800e2-103">Esta explicação passo a passo mostra como usar o Entity Framework Designer (EF designer) para importar procedimentos armazenados em um modelo e, em seguida, chamar os procedimentos armazenados importados para recuperar resultados.</span><span class="sxs-lookup"><span data-stu-id="800e2-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="800e2-104">Observe que Code First não dá suporte ao mapeamento para procedimentos armazenados ou funções.</span><span class="sxs-lookup"><span data-stu-id="800e2-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="800e2-105">No entanto, você pode chamar procedimentos armazenados ou funções usando o método System. Data. Entity. DbSet. SQLQuery.</span><span class="sxs-lookup"><span data-stu-id="800e2-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="800e2-106">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="800e2-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="800e2-107">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="800e2-107">Prerequisites</span></span>

<span data-ttu-id="800e2-108">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="800e2-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="800e2-109">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="800e2-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="800e2-110">O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="800e2-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="800e2-111">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="800e2-111">Set up the Project</span></span>

-   <span data-ttu-id="800e2-112">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="800e2-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="800e2-113">Selecione **arquivo-&gt; Projeto New-&gt;**</span><span class="sxs-lookup"><span data-stu-id="800e2-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="800e2-114">No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console** .</span><span class="sxs-lookup"><span data-stu-id="800e2-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="800e2-115">Insira **EFwithSProcsSample** As o nome.</span><span class="sxs-lookup"><span data-stu-id="800e2-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="800e2-116">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="800e2-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="800e2-117">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="800e2-117">Create a Model</span></span>

-   <span data-ttu-id="800e2-118">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.</span><span class="sxs-lookup"><span data-stu-id="800e2-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="800e2-119">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="800e2-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="800e2-120">Digite **EFwithSProcsModel. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="800e2-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="800e2-121">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="800e2-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="800e2-122">Clique em **nova conexão**.</span><span class="sxs-lookup"><span data-stu-id="800e2-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="800e2-123">Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB) \\mssqllocaldb**), selecione o método de autenticação, digite **School** for o nome do banco de dados e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="800e2-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="800e2-124">A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="800e2-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="800e2-125">Na caixa de diálogo escolher seu objeto de banco de dados, verifique as **tabelas** checkbox para selecionar todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="800e2-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="800e2-126">Além disso, selecione os seguintes procedimentos armazenados no nó **procedimentos armazenados e funções** : **GetStudentGrades** e **getdepartmentname**.</span><span class="sxs-lookup"><span data-stu-id="800e2-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Importar](~/ef6/media/import.jpg)

    <span data-ttu-id="800e2-128">*Starting com o Visual Studio 2012, o designer do EF dá suporte à importação em massa de procedimentos armazenados. A **importação de procedimentos armazenados e funções selecionados para o modelo de entidade** é marcada por padrão.*</span><span class="sxs-lookup"><span data-stu-id="800e2-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="800e2-129">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="800e2-129">Click **Finish**.</span></span>

<span data-ttu-id="800e2-130">Por padrão, a forma de resultado de cada procedimento armazenado importado ou função que retorna mais de uma coluna se tornará automaticamente um novo tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="800e2-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="800e2-131">Neste exemplo, queremos mapear os resultados da função **GetStudentGrades** para a entidade **StudentGrade** e os resultados de **getdepartmentname** para **None** (**None** é o valor padrão).</span><span class="sxs-lookup"><span data-stu-id="800e2-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="800e2-132">Para uma importação de função retornar um tipo de entidade, as colunas retornadas pelo procedimento armazenado correspondente devem corresponder exatamente às propriedades escalares do tipo de entidade retornado.</span><span class="sxs-lookup"><span data-stu-id="800e2-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="800e2-133">Uma importação de função também pode retornar coleções de tipos simples, tipos complexos ou nenhum valor.</span><span class="sxs-lookup"><span data-stu-id="800e2-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="800e2-134">Clique com o botão direito do mouse na superfície de design e selecione **navegador de modelos**.</span><span class="sxs-lookup"><span data-stu-id="800e2-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="800e2-135">No **navegador de modelos**, selecione **importações de função**e clique duas vezes na função **GetStudentGrades** .</span><span class="sxs-lookup"><span data-stu-id="800e2-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="800e2-136">Na caixa de diálogo Editar importação de função, selecione **entidades** And escolha **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="800e2-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="800e2-137">@no__t-a caixa de seleção **importação de função 0The é combinável** na parte superior da caixa de diálogo **importações de função** permitirá que você mapeie para funções combináveis. Se você marcar essa caixa, somente as funções combináveis (funções com valor de tabela) aparecerão na lista suspensa **procedimento armazenado/nome da função** . Se você não marcar essa caixa, somente as funções não combináveis serão mostradas na lista. \*</span><span class="sxs-lookup"><span data-stu-id="800e2-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="800e2-138">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="800e2-138">Use the Model</span></span>

<span data-ttu-id="800e2-139">Abra o arquivo **Program.cs** em que o método **Main** está definido.</span><span class="sxs-lookup"><span data-stu-id="800e2-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="800e2-140">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="800e2-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="800e2-141">O código chama dois procedimentos armazenados: **GetStudentGrades** (retorna **StudentGrades** para a *StudentId*especificada) e **getdepartmentname** (retorna o nome do departamento no parâmetro de saída).</span><span class="sxs-lookup"><span data-stu-id="800e2-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="800e2-142">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="800e2-142">Compile and run the application.</span></span> <span data-ttu-id="800e2-143">O programa produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="800e2-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="800e2-144">Parâmetros de saída</span><span class="sxs-lookup"><span data-stu-id="800e2-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="800e2-145">Se os parâmetros de saída forem usados, seus valores não estarão disponíveis até que os resultados tenham sido completamente lidos.</span><span class="sxs-lookup"><span data-stu-id="800e2-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="800e2-146">Isso ocorre devido ao comportamento subjacente de DbDataReader, consulte [recuperando dados usando um DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="800e2-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
