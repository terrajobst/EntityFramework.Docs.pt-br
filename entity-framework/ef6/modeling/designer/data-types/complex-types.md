---
title: Tipos complexos - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489902"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="ab7d6-102">Tipos complexos - EF Designer</span><span class="sxs-lookup"><span data-stu-id="ab7d6-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="ab7d6-103">Este tópico mostra como mapear tipos complexos com o Entity Framework Designer (Designer de EF) e como consultar entidades que contêm as propriedades de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="ab7d6-104">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="ab7d6-106">Quando você compila o modelo conceitual, os avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="ab7d6-107">Você pode ignorar esses avisos porque depois que você optar por gerar o banco de dados do modelo, os erros serão eliminados.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="ab7d6-108">O que é um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-108">What is a Complex Type</span></span>

<span data-ttu-id="ab7d6-109">Tipos complexos são propriedades não escalares de tipos de entidade que permitem que propriedades escalares sejam organizadas dentro das entidades.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="ab7d6-110">Como as entidades, tipos complexos consistem em propriedades escalares ou outras propriedades de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="ab7d6-111">Quando você trabalha com objetos que representam tipos complexos, esteja ciente das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="ab7d6-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="ab7d6-112">Tipos complexos não têm chaves e, portanto, não podem existir independente.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="ab7d6-113">Tipos complexos podem existir somente como propriedades de tipos de entidade ou outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="ab7d6-114">Tipos complexos não podem participar de associações e não podem conter propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="ab7d6-115">Propriedades de tipo complexo não podem ser **nulo**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="ab7d6-116">Um \* \* InvalidOperationException \* \* ocorre quando **SaveChanges** é chamado e um objeto complexo nulo é encontrado.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="ab7d6-117">As propriedades escalares de objetos complexos podem ser **nulo**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="ab7d6-118">Tipos complexos não podem herdar de outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="ab7d6-119">Você deve definir o tipo complexo como uma **classe**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="ab7d6-120">O EF detecta alterações nos membros em um objeto de tipo complexo quando **DbContext.DetectChanges** é chamado.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="ab7d6-121">Entity Framework chama **DetectChanges** automaticamente quando os seguintes membros são chamados: **DbSet**, **DbSet**, **DbSet.Remove**, **DbSet**, **entenderão**, **SaveChanges**, **DbContext.GetValidationErrors**, **Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="ab7d6-122">Refatorar as propriedades de uma entidade no novo tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="ab7d6-123">Se você já tiver uma entidade no modelo conceitual que talvez você queira refatorar algumas das propriedades em uma propriedade de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="ab7d6-124">Na superfície do designer, selecione um ou mais propriedades (exceto as propriedades de navegação) de uma entidade, em seguida, clique com botão direito e selecione **refatorar -&gt; mover para o novo tipo complexo**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refatoração](~/ef6/media/refactor.png)

<span data-ttu-id="ab7d6-126">Um novo tipo complexo com as propriedades selecionadas é adicionado para o **Model Browser**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="ab7d6-127">O tipo complexo recebe um nome padrão.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-127">The complex type is given a default name.</span></span>

<span data-ttu-id="ab7d6-128">Uma propriedade complexa do tipo recém-criado substitui as propriedades selecionadas.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="ab7d6-129">Todos os mapeamentos de propriedade são preservados.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-129">All property mappings are preserved.</span></span>

![Refatorar 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="ab7d6-131">Criar um novo tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-131">Create a New Complex Type</span></span>

<span data-ttu-id="ab7d6-132">Você também pode criar um novo tipo complexo que contém as propriedades de uma entidade existente.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="ab7d6-133">Clique com botão direito do **tipos complexos** pasta no navegador de modelo, aponte para **tipo complexo de AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="ab7d6-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="ab7d6-134">Como alternativa, você pode selecionar o **tipos complexos** pasta e pressione a **inserir** em seu teclado.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Adicionar um tipo complexo de novo](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="ab7d6-136">Um novo tipo complexo é adicionado à pasta com um nome padrão.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="ab7d6-137">Agora você pode adicionar propriedades ao tipo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="ab7d6-138">Adicionar propriedades a um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="ab7d6-139">Propriedades de um tipo complexo podem ser tipos escalares ou tipos complexos existentes.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="ab7d6-140">No entanto, as propriedades de tipo complexo não podem ter referências circulares.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="ab7d6-141">Por exemplo, um tipo complexo **OnsiteCourseDetails** não pode ter uma propriedade de tipo complexo **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="ab7d6-142">Você pode adicionar uma propriedade para um tipo complexo em qualquer um dos modos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="ab7d6-143">Um tipo complexo no modelo de navegador com o botão direito, aponte para **Add**, aponte para **propriedade escalar** ou **propriedade complexa**, em seguida, selecione o tipo de propriedade desejada.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="ab7d6-144">Como alternativa, você pode selecionar um tipo complexo e, em seguida, pressione a **inserir** em seu teclado.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Adicionar propriedades ao tipo complexo](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="ab7d6-146">Uma nova propriedade é adicionada para o tipo complexo com um nome padrão.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="ab7d6-147">OU-</span><span class="sxs-lookup"><span data-stu-id="ab7d6-147">OR -</span></span>

-   <span data-ttu-id="ab7d6-148">Com o botão direito em uma propriedade de entidade na **EF Designer** superfície e selecione **cópia**, em seguida, clique com botão direito no tipo complexo a **Model Browser** e selecione  **Colar**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="ab7d6-149">Renomear um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-149">Rename a Complex Type</span></span>

<span data-ttu-id="ab7d6-150">Quando você renomear um tipo complexo, todas as referências para o tipo são atualizadas em todo o projeto.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="ab7d6-151">Lentamente clique duas vezes em um tipo complexo na **Model Browser**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="ab7d6-152">O nome será selecionado e em modo de edição.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="ab7d6-153">OU-</span><span class="sxs-lookup"><span data-stu-id="ab7d6-153">OR -</span></span>

-   <span data-ttu-id="ab7d6-154">Um tipo complexo em com o botão direito do **Model Browser** e selecione **Renomear**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="ab7d6-155">OU-</span><span class="sxs-lookup"><span data-stu-id="ab7d6-155">OR -</span></span>

-   <span data-ttu-id="ab7d6-156">Selecione um tipo complexo no modelo de navegador e pressione a tecla F2.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="ab7d6-157">OU-</span><span class="sxs-lookup"><span data-stu-id="ab7d6-157">OR -</span></span>

-   <span data-ttu-id="ab7d6-158">Um tipo complexo em com o botão direito do **Model Browser** e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="ab7d6-159">Editar o nome na **propriedades** janela.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="ab7d6-160">Adicionar um tipo complexo existente a uma entidade e mapeie suas propriedades para colunas de tabela</span><span class="sxs-lookup"><span data-stu-id="ab7d6-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="ab7d6-161">Uma entidade com o botão direito, aponte para **adicionar novo**e selecione **propriedade complexa**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="ab7d6-162">Uma propriedade de tipo complexo com um nome padrão é adicionada à entidade.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="ab7d6-163">Um tipo de padrão (escolhido dentre os tipos complexos existentes) é atribuído à propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="ab7d6-164">Atribuir o tipo desejado para a propriedade na **propriedades** janela.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="ab7d6-165">Depois de adicionar uma propriedade de tipo complexo para uma entidade, você deve mapear suas propriedades para colunas de tabela.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="ab7d6-166">Clique com botão direito na superfície de design ou em um tipo de entidade de **Model Browser** e selecione **mapeamentos de tabela**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="ab7d6-167">Os mapeamentos de tabela são exibidos na **Mapping Details** janela.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="ab7d6-168">Expanda o **mapeia para &lt;nome da tabela&gt;**  nó.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="ab7d6-169">Um **mapeamentos de coluna** nó é exibido.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="ab7d6-170">Expanda o **mapeamentos de coluna** nó.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="ab7d6-171">É exibida uma lista de todas as colunas na tabela.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="ab7d6-172">As propriedades padrão (se houver) que mapeiam as colunas são listados sob o **propriedade/valor** título.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="ab7d6-173">Selecione a coluna que você deseja mapear e, em seguida, clique com botão direito correspondente **propriedade/valor** campo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="ab7d6-174">Uma lista suspensa de todas as propriedades escalares é exibida.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="ab7d6-175">Selecione a propriedade apropriada.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-175">Select the appropriate property.</span></span>

    ![Tipo complexo de mapa](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="ab7d6-177">Repita as etapas 6 e 7 para cada coluna de tabela.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="ab7d6-178">Para excluir um mapeamento de coluna, selecione a coluna que você deseja mapear e, em seguida, clique no **propriedade/valor** campo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="ab7d6-179">Em seguida, selecione **excluir** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="ab7d6-180">Mapear uma importação de função para um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="ab7d6-181">Importações de função são baseadas em procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="ab7d6-182">Para mapear uma importação de função para um tipo complexo, as colunas retornadas pelo procedimento armazenado correspondente devem coincidir com as propriedades do tipo complexo em número e devem ter tipos de armazenamento que são compatíveis com os tipos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="ab7d6-183">Clique duas vezes em uma função importada que você deseja mapear para um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Importações de função](~/ef6/media/functionimports.png)

-   <span data-ttu-id="ab7d6-185">Preencha as configurações para a nova importação de função, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ab7d6-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="ab7d6-186">Especifique o procedimento armazenado para o qual você está criando uma importação de função do **nome do procedimento armazenado** campo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="ab7d6-187">Este campo é uma lista suspensa que exibe todos os procedimentos armazenados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="ab7d6-188">Especifique o nome da função de importação na **nome da função importar** campo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="ab7d6-189">Selecione **complexos** como o retorno de tipo e, em seguida, especifique o tipo de retorno complexo específico, escolhendo o tipo apropriado na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Editar função de importação](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="ab7d6-191">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-191">Click **OK**.</span></span>
    <span data-ttu-id="ab7d6-192">A entrada de importação de função é criada no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="ab7d6-193">Personalizar o mapeamento para a importação de função de coluna</span><span class="sxs-lookup"><span data-stu-id="ab7d6-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="ab7d6-194">A importação de função no modelo de navegador com o botão direito e selecione **Importar mapeamento de função**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="ab7d6-195">O **detalhes de mapeamento** aparece e mostra o mapeamento padrão para a importação de função.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="ab7d6-196">As setas indicam os mapeamentos entre valores de coluna e valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="ab7d6-197">Por padrão, os nomes de coluna devem para ser o mesmo que os nomes de propriedade do tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="ab7d6-198">Os nomes de coluna padrão são exibidos em texto cinza.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="ab7d6-199">Se necessário, altere os nomes de coluna para coincidir com os nomes de coluna são retornados pelo procedimento armazenado que corresponde à importação de função.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="ab7d6-200">Excluir um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-200">Delete a Complex Type</span></span>

<span data-ttu-id="ab7d6-201">Quando você exclui um tipo complexo, o tipo é excluído do modelo conceitual e mapeamentos de todas as instâncias do tipo são excluídos.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="ab7d6-202">No entanto, as referências para o tipo não são atualizadas.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-202">However, references to the type are not updated.</span></span> <span data-ttu-id="ab7d6-203">Por exemplo, se uma entidade tem uma propriedade de tipo complexo do tipo ComplexType1 e ComplexType1 for excluído na **Model Browser**, a propriedade de entidade correspondente não é atualizada.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="ab7d6-204">O modelo não validará porque ela contém uma entidade que faz referência a um tipo complexo excluído.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="ab7d6-205">Você pode atualizar ou excluir as referências a tipos complexos excluídos usando o Designer de entidade.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="ab7d6-206">Um tipo complexo no modelo de navegador com o botão direito e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="ab7d6-207">OU-</span><span class="sxs-lookup"><span data-stu-id="ab7d6-207">OR -</span></span>

-   <span data-ttu-id="ab7d6-208">Selecione um tipo complexo no modelo de navegador e pressione a tecla Delete no teclado.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="ab7d6-209">Consultar entidades que contém as propriedades de tipo complexo</span><span class="sxs-lookup"><span data-stu-id="ab7d6-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="ab7d6-210">O código a seguir mostra como executar uma consulta que retorna uma coleção de entidade em objetos do tipo que contêm uma propriedade de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="ab7d6-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
