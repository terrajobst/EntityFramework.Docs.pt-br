---
title: Tipos complexos-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418647"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="a5d8b-102">Tipos complexos – designer de EF</span><span class="sxs-lookup"><span data-stu-id="a5d8b-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="a5d8b-103">Este tópico mostra como mapear tipos complexos com o Entity Framework Designer (EF designer) e como consultar entidades que contêm propriedades de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="a5d8b-104">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="a5d8b-106">Quando você cria o modelo conceitual, os avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="a5d8b-107">Você pode ignorar esses avisos porque depois de optar por gerar o banco de dados do modelo, os erros serão desaparecedos.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="a5d8b-108">O que é um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-108">What is a Complex Type</span></span>

<span data-ttu-id="a5d8b-109">Tipos complexos são propriedades não escalares de tipos de entidade que permitem que propriedades escalares sejam organizadas dentro das entidades.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="a5d8b-110">Como as entidades, os tipos complexos consistem em Propriedades escalares ou outras propriedades de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="a5d8b-111">Quando você trabalha com objetos que representam tipos complexos, esteja atento ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="a5d8b-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="a5d8b-112">Tipos complexos não têm chaves e, portanto, não podem existir de forma independente.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="a5d8b-113">Tipos complexos só podem existir como propriedades de tipos de entidade ou outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="a5d8b-114">Tipos complexos não podem participar de associações e não podem conter Propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="a5d8b-115">As propriedades de tipo complexo não podem ser **nulas**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="a5d8b-116">Um **InvalidOperationException **ocorre quando **DbContext. SaveChanges** é chamado e um objeto complexo nulo é encontrado.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="a5d8b-117">As propriedades escalares de objetos complexos podem ser **nulas**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="a5d8b-118">Tipos complexos não podem herdar de outros tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="a5d8b-119">Você deve definir o tipo complexo como uma **classe**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="a5d8b-120">O EF detecta alterações nos membros de um objeto de tipo complexo quando **DbContext. DetectChanges** é chamado.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="a5d8b-121">Entity Framework chama **DetectChanges** automaticamente quando os seguintes membros são chamados: **DbSet. Find**, **DbSet. local**, **DbSet. Remove**, **DbSet. Adicionar**, **DbSet. Attach**, **DbContext. SaveChanges**, **DbContext. getvalidationerrors**, **DbContext. entry**, **DbChangeTracker. Entries**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="a5d8b-122">Refatorar as propriedades de uma entidade em um novo tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="a5d8b-123">Se você já tiver uma entidade em seu modelo conceitual, talvez queira refatorar algumas das propriedades em uma propriedade de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="a5d8b-124">Na superfície do designer, selecione uma ou mais Propriedades (excluindo as propriedades de navegação) de uma entidade, clique com o botão direito do mouse e selecione **refatorar-&gt; mover para o novo tipo complexo**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refatoração](~/ef6/media/refactor.png)

<span data-ttu-id="a5d8b-126">Um novo tipo complexo com as propriedades selecionadas é adicionado ao **navegador de modelo**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="a5d8b-127">O tipo complexo recebe um nome padrão.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-127">The complex type is given a default name.</span></span>

<span data-ttu-id="a5d8b-128">Uma propriedade complexa do tipo recém-criado substitui as propriedades selecionadas.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="a5d8b-129">Todos os mapeamentos de propriedade são preservados.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-129">All property mappings are preserved.</span></span>

![Refatorar 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="a5d8b-131">Criar um novo tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-131">Create a New Complex Type</span></span>

<span data-ttu-id="a5d8b-132">Você também pode criar um novo tipo complexo que não contenha propriedades de uma entidade existente.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="a5d8b-133">Clique com o botão direito do mouse na pasta **tipos complexos** no navegador de modelos, aponte para **tipo complexo AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="a5d8b-134">Como alternativa, você pode selecionar a pasta **tipos complexos** e pressionar a tecla **Insert** no teclado.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Adicionar novo tipo complexo](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="a5d8b-136">Um novo tipo complexo é adicionado à pasta com um nome padrão.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="a5d8b-137">Agora você pode adicionar propriedades ao tipo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="a5d8b-138">Adicionar propriedades a um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="a5d8b-139">As propriedades de um tipo complexo podem ser tipos escalares ou tipos complexos existentes.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="a5d8b-140">No entanto, as propriedades de tipo complexo não podem ter referências circulares.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="a5d8b-141">Por exemplo, um tipo complexo **OnsiteCourseDetails** não pode ter uma propriedade de tipo complexo **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="a5d8b-142">Você pode adicionar uma propriedade a um tipo complexo em qualquer uma das maneiras listadas abaixo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="a5d8b-143">Clique com o botão direito do mouse em um tipo complexo no navegador de modelos, aponte para **Adicionar**, aponte para **Propriedade escalar** ou **Propriedade complexa**e selecione o tipo de propriedade desejado.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="a5d8b-144">Como alternativa, você pode selecionar um tipo complexo e pressionar a tecla **Insert** no teclado.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Adicionar propriedades ao tipo complexo](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="a5d8b-146">Uma nova propriedade é adicionada ao tipo complexo com um nome padrão.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="a5d8b-147">OU –</span><span class="sxs-lookup"><span data-stu-id="a5d8b-147">OR -</span></span>

-   <span data-ttu-id="a5d8b-148">Clique com o botão direito do mouse em uma propriedade de entidade na superfície do **designer do EF** e selecione **copiar**, clique com o botão direito do mouse no tipo complexo no **navegador modelo** e selecione **colar**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="a5d8b-149">Renomear um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-149">Rename a Complex Type</span></span>

<span data-ttu-id="a5d8b-150">Quando você renomeia um tipo complexo, todas as referências ao tipo são atualizadas em todo o projeto.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="a5d8b-151">Clique duas vezes em um tipo complexo no **navegador de modelos**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="a5d8b-152">O nome será selecionado e no modo de edição.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="a5d8b-153">OU –</span><span class="sxs-lookup"><span data-stu-id="a5d8b-153">OR -</span></span>

-   <span data-ttu-id="a5d8b-154">Clique com o botão direito do mouse em um tipo complexo no **navegador de modelos** e selecione **renomear**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="a5d8b-155">OU –</span><span class="sxs-lookup"><span data-stu-id="a5d8b-155">OR -</span></span>

-   <span data-ttu-id="a5d8b-156">Selecione um tipo complexo no navegador de modelos e pressione a tecla F2.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="a5d8b-157">OU –</span><span class="sxs-lookup"><span data-stu-id="a5d8b-157">OR -</span></span>

-   <span data-ttu-id="a5d8b-158">Clique com o botão direito do mouse em um tipo complexo no **navegador de modelos** e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="a5d8b-159">Edite o nome na janela **propriedades** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="a5d8b-160">Adicionar um tipo complexo existente a uma entidade e mapear suas propriedades para colunas de tabela</span><span class="sxs-lookup"><span data-stu-id="a5d8b-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="a5d8b-161">Clique com o botão direito do mouse em uma entidade, aponte para **Adicionar nova**e selecione **Propriedade complexa**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="a5d8b-162">Uma propriedade de tipo complexo com um nome padrão é adicionada à entidade.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="a5d8b-163">Um tipo padrão (escolhido dos tipos complexos existentes) é atribuído à propriedade.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="a5d8b-164">Atribua o tipo desejado à propriedade na janela de **Propriedades** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="a5d8b-165">Depois de adicionar uma propriedade de tipo complexo a uma entidade, você deve mapear suas propriedades para colunas de tabela.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="a5d8b-166">Clique com o botão direito do mouse em um tipo de entidade na superfície de design ou no **navegador de modelos** e selecione **mapeamentos de tabela**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="a5d8b-167">Os mapeamentos de tabela são exibidos na janela **detalhes do mapeamento** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="a5d8b-168">Expanda os **mapas para &lt;nome da tabela&gt;** nó .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="a5d8b-169">Um nó **mapeamentos de coluna** é exibido.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="a5d8b-170">Expanda os **mapeamentos de coluna** nó.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="a5d8b-171">É exibida uma lista de todas as colunas na tabela.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="a5d8b-172">As propriedades padrão (se houver) às quais o mapa de colunas são listadas no título **valor/propriedade** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="a5d8b-173">Selecione a coluna que você deseja mapear e clique com o botão direito do mouse no campo **valor/propriedade** correspondente.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="a5d8b-174">Uma lista suspensa de todas as propriedades escalares é exibida.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="a5d8b-175">Selecione a propriedade apropriada.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-175">Select the appropriate property.</span></span>

    ![Mapear tipo complexo](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="a5d8b-177">Repita as etapas 6 e 7 para cada coluna de tabela.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="a5d8b-178">Para excluir um mapeamento de coluna, selecione a coluna que você deseja mapear e, em seguida, clique no campo **valor/propriedade** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="a5d8b-179">Em seguida, selecione **excluir** na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="a5d8b-180">Mapear uma importação de função para um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="a5d8b-181">As importações de função são baseadas em procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="a5d8b-182">Para mapear uma importação de função para um tipo complexo, as colunas retornadas pelo procedimento armazenado correspondente devem corresponder às propriedades do tipo complexo em número e devem ter tipos de armazenamento compatíveis com os tipos de propriedade.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="a5d8b-183">Clique duas vezes em uma função importada que você deseja mapear para um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Importações de função](~/ef6/media/functionimports.png)

-   <span data-ttu-id="a5d8b-185">Preencha as configurações para a nova importação de função, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="a5d8b-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="a5d8b-186">Especifique o procedimento armazenado para o qual você está criando uma importação de função no campo **nome do procedimento armazenado** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="a5d8b-187">Esse campo é uma lista suspensa que exibe todos os procedimentos armazenados no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="a5d8b-188">Especifique o nome da função de importação no campo **nome da importação de função** .</span><span class="sxs-lookup"><span data-stu-id="a5d8b-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="a5d8b-189">Selecione  **complexas** como o tipo de retorno e, em seguida, especifique o tipo de retorno complexo específico escolhendo o tipo apropriado na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Editar importação de função](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="a5d8b-191">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-191">Click **OK**.</span></span>
    <span data-ttu-id="a5d8b-192">A entrada de importação de função é criada no modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="a5d8b-193">Personalizar o mapeamento de coluna para importação de função</span><span class="sxs-lookup"><span data-stu-id="a5d8b-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="a5d8b-194">Clique com o botão direito do mouse na função importar no navegador de modelos e selecione **função importar mapeamento**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="a5d8b-195">A janela **detalhes do mapeamento** é exibida e mostra o mapeamento padrão para a importação de função.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="a5d8b-196">As setas indicam os mapeamentos entre valores de coluna e valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="a5d8b-197">Por padrão, supõe-se que os nomes de coluna sejam iguais aos nomes de Propriedade do tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="a5d8b-198">Os nomes de coluna padrão aparecem em texto cinza.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="a5d8b-199">Se necessário, altere os nomes de coluna para coincidir com os nomes de coluna retornados pelo procedimento armazenado que corresponde à importação de função.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="a5d8b-200">Excluir um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-200">Delete a Complex Type</span></span>

<span data-ttu-id="a5d8b-201">Quando você exclui um tipo complexo, o tipo é excluído do modelo conceitual e os mapeamentos para todas as instâncias do tipo são excluídos.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="a5d8b-202">No entanto, as referências ao tipo não são atualizadas.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-202">However, references to the type are not updated.</span></span> <span data-ttu-id="a5d8b-203">Por exemplo, se uma entidade tiver uma propriedade de tipo complexo do tipo ComplexType1 e ComplexType1 for excluído no **navegador de modelo**, a propriedade de entidade correspondente não será atualizada.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="a5d8b-204">O modelo não será validado porque contém uma entidade que faz referência a um tipo complexo excluído.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="a5d8b-205">Você pode atualizar ou excluir referências para tipos complexos excluídos usando o Entity Designer.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="a5d8b-206">Clique com o botão direito do mouse em um tipo complexo no navegador de modelos e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="a5d8b-207">OU –</span><span class="sxs-lookup"><span data-stu-id="a5d8b-207">OR -</span></span>

-   <span data-ttu-id="a5d8b-208">Selecione um tipo complexo no navegador de modelos e pressione a tecla Delete no teclado.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="a5d8b-209">Consulta de entidades que contêm propriedades de tipo complexo</span><span class="sxs-lookup"><span data-stu-id="a5d8b-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="a5d8b-210">O código a seguir mostra como executar uma consulta que retorna uma coleção de objetos de tipo entidade que contêm uma propriedade de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a5d8b-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
