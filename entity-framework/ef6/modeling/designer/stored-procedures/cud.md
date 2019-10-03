---
title: Procedimentos armazenados do designer CUD – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813557"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="0c090-102">Procedimentos armazenados do designer CUD</span><span class="sxs-lookup"><span data-stu-id="0c090-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="0c090-103">Esta explicação passo a passo mostra como mapear as operações Create @ no__t-0insert, Update e Delete (CUD) de um tipo de entidade para procedimentos armazenados usando o Entity Framework Designer (EF designer).</span><span class="sxs-lookup"><span data-stu-id="0c090-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="0c090-104"> Por padrão, o Entity Framework gera automaticamente as instruções SQL para as operações CUD, mas você também pode mapear procedimentos armazenados para essas operações.</span><span class="sxs-lookup"><span data-stu-id="0c090-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="0c090-105">Observe que Code First não dá suporte ao mapeamento para procedimentos armazenados ou funções.</span><span class="sxs-lookup"><span data-stu-id="0c090-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="0c090-106">No entanto, você pode chamar procedimentos armazenados ou funções usando o método System. Data. Entity. DbSet. SQLQuery.</span><span class="sxs-lookup"><span data-stu-id="0c090-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="0c090-107">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0c090-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="0c090-108">Considerações ao mapear as operações de CUD para procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="0c090-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="0c090-109">Ao mapear as operações de CUD para procedimentos armazenados, as seguintes considerações se aplicam:</span><span class="sxs-lookup"><span data-stu-id="0c090-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="0c090-110">Se você estiver mapeando uma das operações CUD para um procedimento armazenado, mapeie todas elas.</span><span class="sxs-lookup"><span data-stu-id="0c090-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="0c090-111">Se você não mapear todos os três, as operações não mapeadas falharão se executadas e uma **updateexception** will ser lançada.</span><span class="sxs-lookup"><span data-stu-id="0c090-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="0c090-112">Você deve mapear todos os parâmetros do procedimento armazenado para as propriedades da entidade.</span><span class="sxs-lookup"><span data-stu-id="0c090-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="0c090-113">Se o servidor gerar o valor de chave primária para a linha inserida, você deverá mapear esse valor de volta para a propriedade de chave da entidade.</span><span class="sxs-lookup"><span data-stu-id="0c090-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="0c090-114">No exemplo a seguir, o procedimento **InsertPerson** stored retorna a chave primária recém-criada como parte do conjunto de resultados do procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="0c090-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="0c090-115">A chave primária é mapeada para a chave de entidade (**PersonID**) usando as **associações de resultado &lt;Add @ no__t-3** FEATURE do designer do EF.</span><span class="sxs-lookup"><span data-stu-id="0c090-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="0c090-116">As chamadas de procedimento armazenado são mapeadas 1:1 com as entidades no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="0c090-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="0c090-117">Por exemplo, se você implementar uma hierarquia de herança em seu modelo conceitual e, em seguida, mapear os procedimentos armazenados do CUD para as entidades **pai** (base) e **filho** (derivado), salvar as alterações **filho** chamará somente o **filho**' os procedimentos armazenados de s, não dispararão as chamadas de procedimentos armazenados do **pai**.</span><span class="sxs-lookup"><span data-stu-id="0c090-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c090-118">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0c090-118">Prerequisites</span></span>

<span data-ttu-id="0c090-119">Para concluir esta explicação passo a passo, será necessário:</span><span class="sxs-lookup"><span data-stu-id="0c090-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="0c090-120">Uma versão recente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c090-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="0c090-121">O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="0c090-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0c090-122">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="0c090-122">Set up the Project</span></span>

- <span data-ttu-id="0c090-123">Abra o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="0c090-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="0c090-124">Selecione **arquivo-&gt; Projeto New-&gt;**</span><span class="sxs-lookup"><span data-stu-id="0c090-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="0c090-125">No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console** .</span><span class="sxs-lookup"><span data-stu-id="0c090-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="0c090-126">Insira **CUDSProcsSample** As o nome.</span><span class="sxs-lookup"><span data-stu-id="0c090-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="0c090-127">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c090-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="0c090-128">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="0c090-128">Create a Model</span></span>

- <span data-ttu-id="0c090-129">Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.</span><span class="sxs-lookup"><span data-stu-id="0c090-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="0c090-130">Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.</span><span class="sxs-lookup"><span data-stu-id="0c090-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="0c090-131">Digite **CUDSProcs. edmx** para o nome do arquivo e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="0c090-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="0c090-132">Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="0c090-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="0c090-133">Clique em **nova conexão**.</span><span class="sxs-lookup"><span data-stu-id="0c090-133">Click **New Connection**.</span></span> <span data-ttu-id="0c090-134">Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(\\LocalDB) mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c090-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="0c090-135">A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0c090-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="0c090-136">Na caixa de diálogo escolher seu objeto de banco de dados, nas **tabelas** node, selecione a tabela **Person** .</span><span class="sxs-lookup"><span data-stu-id="0c090-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="0c090-137">Além disso, selecione os seguintes procedimentos armazenados no nó **procedimentos armazenados e funções** : **DeletePerson**, **InsertPerson**e **UpdatePerson**.</span><span class="sxs-lookup"><span data-stu-id="0c090-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="0c090-138">A partir do Visual Studio 2012, o designer do EF dá suporte à importação em massa de procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="0c090-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="0c090-139">As **funções e os procedimentos armazenados selecionados no modelo de entidade** são verificados por padrão.</span><span class="sxs-lookup"><span data-stu-id="0c090-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="0c090-140">Como neste exemplo temos procedimentos armazenados que inserem, atualizam e excluem tipos de entidade, não queremos importá-los e Desmarcarei essa caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="0c090-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![Importar os procs](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="0c090-142">Clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="0c090-142">Click **Finish**.</span></span>
    <span data-ttu-id="0c090-143">O designer do EF, que fornece uma superfície de design para editar seu modelo, é exibido.</span><span class="sxs-lookup"><span data-stu-id="0c090-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="0c090-144">Mapear a entidade Person para procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="0c090-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="0c090-145">Clique com o botão direito do mouse no tipo **Person** entity e selecione **mapeamento de procedimento armazenado**.</span><span class="sxs-lookup"><span data-stu-id="0c090-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="0c090-146">Os mapeamentos de procedimento armazenado são exibidos nos **detalhes de mapeamento** window.</span><span class="sxs-lookup"><span data-stu-id="0c090-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="0c090-147">Clique em **&lt;Select inserir função @ no__t-2**.</span><span class="sxs-lookup"><span data-stu-id="0c090-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="0c090-148">O campo torna-se uma lista suspensa dos procedimentos armazenados no modelo de armazenamento que podem ser mapeados para tipos de entidade no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="0c090-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="0c090-149">Selecione **InsertPerson** from a lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="0c090-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="0c090-150">Os mapeamentos padrão entre parâmetros de procedimento armazenado e propriedades de entidade são exibidos.</span><span class="sxs-lookup"><span data-stu-id="0c090-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="0c090-151">Observe que as setas indicam a direção do mapeamento: Os valores de propriedade são fornecidos para parâmetros de procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="0c090-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="0c090-152">Clique em **Associação de resultado &lt;Add @ no__t-2**.</span><span class="sxs-lookup"><span data-stu-id="0c090-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="0c090-153">Digite **NewPersonID**, o nome do parâmetro retornado pelo procedimento **InsertPerson** stored.</span><span class="sxs-lookup"><span data-stu-id="0c090-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="0c090-154">Certifique-se de não digitar espaços à esquerda ou à direita.</span><span class="sxs-lookup"><span data-stu-id="0c090-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="0c090-155">Pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="0c090-155">Press **Enter**.</span></span>
- <span data-ttu-id="0c090-156">Por padrão, **NewPersonID** is mapeado para a chave de entidade **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="0c090-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="0c090-157">Observe que uma seta indica a direção do mapeamento: O valor da coluna de resultado é fornecido para a propriedade.</span><span class="sxs-lookup"><span data-stu-id="0c090-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Detalhes do mapeamento](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="0c090-159">Clique em **&lt;Select função de atualização @ no__t-2** and selecione **UpdatePerson** from a lista suspensa resultante.</span><span class="sxs-lookup"><span data-stu-id="0c090-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="0c090-160">Os mapeamentos padrão entre parâmetros de procedimento armazenado e propriedades de entidade são exibidos.</span><span class="sxs-lookup"><span data-stu-id="0c090-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="0c090-161">Clique em **&lt;Select excluir função @ no__t-2** and selecione **DeletePerson** from a lista suspensa resultante.</span><span class="sxs-lookup"><span data-stu-id="0c090-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="0c090-162">Os mapeamentos padrão entre parâmetros de procedimento armazenado e propriedades de entidade são exibidos.</span><span class="sxs-lookup"><span data-stu-id="0c090-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="0c090-163">As operações de inserção, atualização e exclusão do tipo **Person** entity agora são mapeadas para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="0c090-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="0c090-164">Se você quiser habilitar a verificação de simultaneidade ao atualizar ou excluir uma entidade com procedimentos armazenados, use uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="0c090-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="0c090-165">Use um parâmetro de **saída** para retornar o número de linhas afetadas do procedimento armazenado e verifique o **parâmetro de linhas afetado** checkbox ao lado do nome do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="0c090-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="0c090-166">Se o valor retornado for zero quando a operação for chamada, um  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will será lançado.</span><span class="sxs-lookup"><span data-stu-id="0c090-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="0c090-167">Marque a caixa de seleção **usar valor original** ao lado de uma propriedade que você deseja usar para verificação de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="0c090-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="0c090-168">Quando uma atualização for tentada, o valor da propriedade que foi originalmente lida do banco de dados será usado ao gravar os dados de volta no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="0c090-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="0c090-169">Se o valor não corresponder ao valor no banco de dados, um **OptimisticConcurrencyException** will será lançado.</span><span class="sxs-lookup"><span data-stu-id="0c090-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="0c090-170">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="0c090-170">Use the Model</span></span>

<span data-ttu-id="0c090-171">Abra o arquivo **Program.cs** em que o método **Main** está definido.</span><span class="sxs-lookup"><span data-stu-id="0c090-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="0c090-172">Adicione o código a seguir à função main.</span><span class="sxs-lookup"><span data-stu-id="0c090-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="0c090-173">O código cria um novo objeto **Person** , depois atualiza o objeto e, por fim, exclui o objeto.</span><span class="sxs-lookup"><span data-stu-id="0c090-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- <span data-ttu-id="0c090-174">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c090-174">Compile and run the application.</span></span> <span data-ttu-id="0c090-175">O programa produz a seguinte saída \*</span><span class="sxs-lookup"><span data-stu-id="0c090-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="0c090-176">PersonID é gerado automaticamente pelo servidor, portanto, você provavelmente verá um número diferente \*</span><span class="sxs-lookup"><span data-stu-id="0c090-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="0c090-177">Se você estiver trabalhando com a versão final do Visual Studio, poderá usar o IntelliTrace com o depurador para ver as instruções SQL que são executadas.</span><span class="sxs-lookup"><span data-stu-id="0c090-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
