---
title: Herança de TPT do designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418206"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="a701c-102">Herança de TPT do designer</span><span class="sxs-lookup"><span data-stu-id="a701c-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="a701c-103">Este guia passo a passo mostra como implementar a herança de tabela por tipo (TPT) em seu modelo usando o Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="a701c-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="a701c-104">A herança de tabela por tipo usa uma tabela separada no banco de dados para manter o dado para propriedades não herdadas e propriedades de chave para cada tipo na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="a701c-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="a701c-105">Neste tutorial, mapearemos o **curso** (tipo base), **OnlineCourse** (deriva de curso) e **OnsiteCourse** (deriva de entidades do **curso**) para tabelas com os mesmos nomes.</span><span class="sxs-lookup"><span data-stu-id="a701c-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="a701c-106">Vamos criar um modelo do banco de dados e, em seguida, alterar o modelo para implementar a herança TPT.</span><span class="sxs-lookup"><span data-stu-id="a701c-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="a701c-107">Você também pode começar com a Model First e, em seguida, gerar o banco de dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="a701c-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="a701c-108">O designer do EF usa a estratégia TPT por padrão e, portanto, qualquer herança no modelo será mapeada para tabelas separadas.</span><span class="sxs-lookup"><span data-stu-id="a701c-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="a701c-109">Outras opções de herança</span><span class="sxs-lookup"><span data-stu-id="a701c-109">Other Inheritance Options</span></span>

<span data-ttu-id="a701c-110">A tabela por hierarquia (TPH) é outro tipo de herança em que uma tabela de banco de dados é usada para manter todos os tipos de entidade em uma hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="a701c-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="a701c-111">  Para obter informações sobre como mapear a herança de tabela por hierarquia com o Entity Designer, consulte [herança do EF designer TPH](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="a701c-111">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="a701c-112">Observe que a herança de tipo de tabela por concreto (TPC) e os modelos de herança misto têm suporte pelo tempo de execução Entity Framework, mas não têm suporte do designer do EF.</span><span class="sxs-lookup"><span data-stu-id="a701c-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="a701c-113">Se você quiser usar o TPC ou a herança mista, terá duas opções: Use Code First ou edite manualmente o arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="a701c-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="a701c-114">Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.</span><span class="sxs-lookup"><span data-stu-id="a701c-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a701c-115">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="a701c-115">Prerequisites</span></span>

<span data-ttu-id="a701c-116">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="a701c-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="a701c-117">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a701c-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="a701c-118">O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="a701c-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a701c-119">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="a701c-119">Set up the Project</span></span>

-   <span data-ttu-id="a701c-120">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a701c-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="a701c-121">Selecione **arquivo-&gt; projeto de novo&gt;**</span><span class="sxs-lookup"><span data-stu-id="a701c-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="a701c-122">No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console** .</span><span class="sxs-lookup"><span data-stu-id="a701c-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="a701c-123">Insira **TPTDBFirstSample** como o nome.</span><span class="sxs-lookup"><span data-stu-id="a701c-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="a701c-124">Selecione  **OK**.</span><span class="sxs-lookup"><span data-stu-id="a701c-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="a701c-125">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="a701c-125">Create a Model</span></span>

-   <span data-ttu-id="a701c-126">Clique com o botão direito do mouse no projeto em Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.</span><span class="sxs-lookup"><span data-stu-id="a701c-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="a701c-127">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="a701c-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="a701c-128">Digite **TPTModel. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="a701c-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="a701c-129">Na caixa de diálogo escolher conteúdo do modelo, selecione ** gerar do banco de dados**e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="a701c-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="a701c-130">Clique em **nova conexão**.</span><span class="sxs-lookup"><span data-stu-id="a701c-130">Click **New Connection**.</span></span>
    <span data-ttu-id="a701c-131">Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a701c-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="a701c-132">A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="a701c-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="a701c-133">Na caixa de diálogo escolher seu objeto de banco de dados, no nó tabelas, selecione as tabelas **Departamento**, **curso, OnlineCourse e OnsiteCourse** .</span><span class="sxs-lookup"><span data-stu-id="a701c-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="a701c-134">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="a701c-134">Click **Finish**.</span></span>

<span data-ttu-id="a701c-135">O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="a701c-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="a701c-136">Todos os objetos que você selecionou na caixa de diálogo escolher seus objetos de banco de dados são adicionados ao modelo.</span><span class="sxs-lookup"><span data-stu-id="a701c-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="a701c-137">Implementar a herança de tabela por tipo</span><span class="sxs-lookup"><span data-stu-id="a701c-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="a701c-138">Na superfície de design, clique com o botão direito do mouse no tipo de entidade **OnlineCourse** e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="a701c-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="a701c-139">Na janela **Propriedades** , defina a propriedade tipo base como **curso**.</span><span class="sxs-lookup"><span data-stu-id="a701c-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="a701c-140">Clique com o botão direito do mouse no tipo de entidade **OnsiteCourse** e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="a701c-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="a701c-141">Na janela **Propriedades** , defina a propriedade tipo base como **curso**.</span><span class="sxs-lookup"><span data-stu-id="a701c-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="a701c-142">Clique com o botão direito do mouse na associação (a linha) entre os tipos de entidade **OnlineCourse** e **curso** .</span><span class="sxs-lookup"><span data-stu-id="a701c-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="a701c-143">Selecione **excluir do modelo**.</span><span class="sxs-lookup"><span data-stu-id="a701c-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="a701c-144">Clique com o botão direito do mouse na associação entre os tipos de entidade **OnsiteCourse** e **curso** .</span><span class="sxs-lookup"><span data-stu-id="a701c-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="a701c-145">Selecione **excluir do modelo**.</span><span class="sxs-lookup"><span data-stu-id="a701c-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="a701c-146">Agora, excluiremos a propriedade **cursoid** de **OnlineCourse** e **OnsiteCourse** porque essas classes herdam o **cursoid** do tipo base do **curso** .</span><span class="sxs-lookup"><span data-stu-id="a701c-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="a701c-147">Clique com o botão direito do mouse na propriedade **cursoid** do tipo de entidade **OnlineCourse** e selecione **excluir do modelo**.</span><span class="sxs-lookup"><span data-stu-id="a701c-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="a701c-148">Clique com o botão direito do mouse na propriedade **cursoid** do tipo de entidade **OnsiteCourse** e selecione **excluir do modelo**</span><span class="sxs-lookup"><span data-stu-id="a701c-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="a701c-149">A herança de tabela por tipo agora está implementada.</span><span class="sxs-lookup"><span data-stu-id="a701c-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="a701c-151">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="a701c-151">Use the Model</span></span>

<span data-ttu-id="a701c-152">Abra o arquivo **Program.cs** em que o método **Main** está definido.</span><span class="sxs-lookup"><span data-stu-id="a701c-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="a701c-153">Cole o código a seguir na função **Main** .</span><span class="sxs-lookup"><span data-stu-id="a701c-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="a701c-154">O código executa três consultas.</span><span class="sxs-lookup"><span data-stu-id="a701c-154">The code executes three queries.</span></span> <span data-ttu-id="a701c-155">A primeira consulta traz de volta todos os **cursos** relacionados ao departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="a701c-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="a701c-156">A segunda consulta usa o método **OfType** para retornar **OnlineCourses** relacionados ao departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="a701c-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="a701c-157">A terceira consulta retorna **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="a701c-157">The third query returns **OnsiteCourses**.</span></span>

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
