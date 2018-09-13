---
title: Herança TPT Designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489447"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="adcf9-102">Herança TPT Designer</span><span class="sxs-lookup"><span data-stu-id="adcf9-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="adcf9-103">Este passo a passo mostra como implementar a herança do tabela por tipo (TPT) em seu modelo usando o Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="adcf9-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="adcf9-104">Herança de tabela por tipo usa uma tabela separada no banco de dados para manter dados de propriedades não herdadas e as propriedades de chave para cada tipo na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="adcf9-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="adcf9-105">Este passo a passo, podemos mapeará os **curso** (tipo de base), **OnlineCourse** (deriva de curso), e **OnsiteCourse** (deriva **curso**) entidades para tabelas com os mesmos nomes.</span><span class="sxs-lookup"><span data-stu-id="adcf9-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="adcf9-106">Vamos criar um modelo do banco de dados e, em seguida, alterar o modelo para implementar a herança TPT.</span><span class="sxs-lookup"><span data-stu-id="adcf9-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="adcf9-107">Você pode também iniciar com o modelo primeiro e, em seguida, gerar o banco de dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="adcf9-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="adcf9-108">O EF Designer usa a estratégia TPT por padrão e então qualquer herança no modelo será mapeada para tabelas separadas.</span><span class="sxs-lookup"><span data-stu-id="adcf9-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="adcf9-109">Outras opções de herança</span><span class="sxs-lookup"><span data-stu-id="adcf9-109">Other Inheritance Options</span></span>

<span data-ttu-id="adcf9-110">Tabela por hierarquia (TPH) é outro tipo de herança em qual banco de dados de uma tabela é usada para manter os dados para todos os tipos de entidade em uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="adcf9-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="adcf9-111">Para obter informações sobre como mapear a herança de tabela por hierarquia com o Entity Designer, consulte [EF Designer TPH herança](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="adcf9-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="adcf9-112">Observe que, o tabela por concreto tipo de herança TPC () e herança mista modelos são compatíveis com o tempo de execução do Entity Framework, mas não são compatíveis com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="adcf9-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="adcf9-113">Se você quiser usar TPC ou herança mista, você tem duas opções: usar o Code First ou editar manualmente o arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="adcf9-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="adcf9-114">Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.</span><span class="sxs-lookup"><span data-stu-id="adcf9-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="adcf9-115">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="adcf9-115">Prerequisites</span></span>

<span data-ttu-id="adcf9-116">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="adcf9-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="adcf9-117">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="adcf9-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="adcf9-118">O [banco de dados de exemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="adcf9-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="adcf9-119">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="adcf9-119">Set up the Project</span></span>

-   <span data-ttu-id="adcf9-120">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="adcf9-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="adcf9-121">Selecione **arquivo -&gt; New -&gt; projeto**</span><span class="sxs-lookup"><span data-stu-id="adcf9-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="adcf9-122">No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.</span><span class="sxs-lookup"><span data-stu-id="adcf9-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="adcf9-123">Insira **TPTDBFirstSample** como o nome.</span><span class="sxs-lookup"><span data-stu-id="adcf9-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="adcf9-124">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="adcf9-125">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="adcf9-125">Create a Model</span></span>

-   <span data-ttu-id="adcf9-126">Clique com botão direito no projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="adcf9-127">Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.</span><span class="sxs-lookup"><span data-stu-id="adcf9-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="adcf9-128">Insira **TPTModel.edmx** para o nome de arquivo e clique **Add**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="adcf9-129">Na caixa de diálogo Escolher conteúdo do modelo caixa, selecione \* \* Gerar do banco de dados \* \* e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-129">In the Choose Model Contents dialog box, select\*\* Generate from database\*\*, and then click **Next**.</span></span>
-   <span data-ttu-id="adcf9-130">Clique em **nova Conexão**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-130">Click **New Connection**.</span></span>
    <span data-ttu-id="adcf9-131">Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="adcf9-132">A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="adcf9-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="adcf9-133">Na caixa de diálogo Choose Your Database Objects, sob o nó de tabelas, selecione a **departamento**, **curso, OnlineCourse e OnsiteCourse** tabelas.</span><span class="sxs-lookup"><span data-stu-id="adcf9-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="adcf9-134">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-134">Click **Finish**.</span></span>

<span data-ttu-id="adcf9-135">O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="adcf9-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="adcf9-136">Todos os objetos que você selecionou na caixa de diálogo Choose Your Database Objects são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="adcf9-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="adcf9-137">Implementar a herança de tabela por tipo</span><span class="sxs-lookup"><span data-stu-id="adcf9-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="adcf9-138">Na superfície de design, clique com botão direito do **OnlineCourse** tipo de entidade e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="adcf9-139">No **propriedades** janela, defina a propriedade de tipo de Base para **curso**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="adcf9-140">Clique com botão direito do **OnsiteCourse** tipo de entidade e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="adcf9-141">No **propriedades** janela, defina a propriedade de tipo de Base para **curso**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="adcf9-142">Clique com botão direito a associação (a linha) entre o **OnlineCourse** e **curso** tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="adcf9-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="adcf9-143">Selecione **excluir do modelo**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="adcf9-144">Clique com botão direito a associação entre o **OnsiteCourse** e **curso** tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="adcf9-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="adcf9-145">Selecione **excluir do modelo**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="adcf9-146">Agora vamos excluir o **CourseID** propriedade de **OnlineCourse** e **OnsiteCourse** porque herdam essas classes **CourseID** de o **curso** tipo base.</span><span class="sxs-lookup"><span data-stu-id="adcf9-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="adcf9-147">Com o botão direito do **CourseID** propriedade da **OnlineCourse** tipo de entidade e, em seguida, selecione **excluir do modelo**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="adcf9-148">Com o botão direito do **CourseID** propriedade da **OnsiteCourse** tipo de entidade e, em seguida, selecione **excluir do modelo**</span><span class="sxs-lookup"><span data-stu-id="adcf9-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="adcf9-149">Herança de tabela por tipo agora é implementada.</span><span class="sxs-lookup"><span data-stu-id="adcf9-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="adcf9-151">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="adcf9-151">Use the Model</span></span>

<span data-ttu-id="adcf9-152">Abra o **Program.cs** do arquivo onde o **Main** método é definido.</span><span class="sxs-lookup"><span data-stu-id="adcf9-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="adcf9-153">Cole o seguinte código para o **Main** função.</span><span class="sxs-lookup"><span data-stu-id="adcf9-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="adcf9-154">O código é executado três consultas.</span><span class="sxs-lookup"><span data-stu-id="adcf9-154">The code executes three queries.</span></span> <span data-ttu-id="adcf9-155">A primeira consulta traz de volta todos os **cursos** relacionadas ao departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="adcf9-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="adcf9-156">A segunda consulta usa a **OfType** método retorne **OnlineCourses** relacionadas ao departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="adcf9-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="adcf9-157">A terceira consulta retorna **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="adcf9-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
