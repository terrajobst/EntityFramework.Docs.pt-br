---
title: Relações-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418241"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="23cb0-102">Relações-designer do EF</span><span class="sxs-lookup"><span data-stu-id="23cb0-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="23cb0-103">Esta página fornece informações sobre como configurar relações em seu modelo usando o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="23cb0-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="23cb0-104">Para obter informações gerais sobre relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="23cb0-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="23cb0-105">As associações definem as relações entre os tipos de entidade em um modelo.</span><span class="sxs-lookup"><span data-stu-id="23cb0-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="23cb0-106">Este tópico mostra como mapear associações com o Entity Framework Designer (EF designer).</span><span class="sxs-lookup"><span data-stu-id="23cb0-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="23cb0-107">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="23cb0-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="23cb0-109">Quando você cria o modelo conceitual, os avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros.</span><span class="sxs-lookup"><span data-stu-id="23cb0-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="23cb0-110">Você pode ignorar esses avisos porque depois de optar por gerar o banco de dados do modelo, os erros serão desaparecedos.</span><span class="sxs-lookup"><span data-stu-id="23cb0-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="23cb0-111">Visão geral de associações</span><span class="sxs-lookup"><span data-stu-id="23cb0-111">Associations Overview</span></span>

<span data-ttu-id="23cb0-112">Quando você cria seu modelo usando o designer do EF, um arquivo. edmx representa seu modelo.</span><span class="sxs-lookup"><span data-stu-id="23cb0-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="23cb0-113">No arquivo. edmx, um elemento **Association** define uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="23cb0-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="23cb0-114">Uma associação deve especificar os tipos de entidade que estão envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como multiplicidade.</span><span class="sxs-lookup"><span data-stu-id="23cb0-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="23cb0-115">A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (0.. 1) ou muitos (\*).</span><span class="sxs-lookup"><span data-stu-id="23cb0-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="23cb0-116">Essas informações são especificadas em dois elementos **finais** filho.</span><span class="sxs-lookup"><span data-stu-id="23cb0-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="23cb0-117">Em tempo de execução, as instâncias de tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras (se você optar por expor chaves estrangeiras em suas entidades).</span><span class="sxs-lookup"><span data-stu-id="23cb0-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="23cb0-118">Com as chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um elemento **ReferentialConstraint** (um elemento filho do elemento **Association** ).</span><span class="sxs-lookup"><span data-stu-id="23cb0-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="23cb0-119">É recomendável que você sempre exponha chaves estrangeiras para relações em suas entidades.</span><span class="sxs-lookup"><span data-stu-id="23cb0-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="23cb0-120">Em muitos para muitos (\*:\*), não é possível adicionar chaves estrangeiras às entidades.</span><span class="sxs-lookup"><span data-stu-id="23cb0-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="23cb0-121">Em uma relação \*:\*, as informações de associação são gerenciadas com um objeto independente.</span><span class="sxs-lookup"><span data-stu-id="23cb0-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="23cb0-122">Para obter informações sobre elementos CSDL (**ReferentialConstraint**, **Association**, etc.), consulte a [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="23cb0-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="23cb0-123">Criar e excluir associações</span><span class="sxs-lookup"><span data-stu-id="23cb0-123">Create and Delete Associations</span></span>

<span data-ttu-id="23cb0-124">A criação de uma associação com o designer do EF atualiza o conteúdo do modelo do arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="23cb0-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="23cb0-125">Depois de criar uma associação, você deve criar os mapeamentos para a associação (discutido posteriormente neste tópico).</span><span class="sxs-lookup"><span data-stu-id="23cb0-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="23cb0-126">Esta seção pressupõe que você já adicionou as entidades para as quais deseja criar uma associação entre o modelo.</span><span class="sxs-lookup"><span data-stu-id="23cb0-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="23cb0-127">Para criar uma associação</span><span class="sxs-lookup"><span data-stu-id="23cb0-127">To create an association</span></span>

1.  <span data-ttu-id="23cb0-128">Clique com o botão direito do mouse em uma área vazia da superfície de design, aponte para **Adicionar nova**e selecione **associação...** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="23cb0-129">Preencha as configurações da associação na caixa de diálogo **Adicionar Associação** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Adicionar Associação](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="23cb0-131">Você pode optar por não adicionar propriedades de navegação ou propriedades de chave estrangeira às entidades nas extremidades da associação, desmarcando a **propriedade de navegação **e **Adicionar propriedades de chave estrangeira ao &lt;nome do tipo de entidade&gt; **caixas de seleção de entidade.</span><span class="sxs-lookup"><span data-stu-id="23cb0-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="23cb0-132">Se você adicionar apenas uma propriedade de navegação, a associação será percorrida em apenas uma direção.</span><span class="sxs-lookup"><span data-stu-id="23cb0-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="23cb0-133">Se você não adicionar nenhuma propriedade de navegação, deverá optar por adicionar propriedades de chave estrangeira para acessar entidades nas extremidades da associação.</span><span class="sxs-lookup"><span data-stu-id="23cb0-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="23cb0-134">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="23cb0-135">Para excluir uma associação</span><span class="sxs-lookup"><span data-stu-id="23cb0-135">To delete an association</span></span>

<span data-ttu-id="23cb0-136">Para excluir uma associação, siga um destes procedimentos:</span><span class="sxs-lookup"><span data-stu-id="23cb0-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="23cb0-137">Clique com o botão direito do mouse na associação na superfície do designer do EF e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="23cb0-138">OU –</span><span class="sxs-lookup"><span data-stu-id="23cb0-138">OR -</span></span>

-   <span data-ttu-id="23cb0-139">Selecione uma ou mais associações e pressione a tecla DELETE.</span><span class="sxs-lookup"><span data-stu-id="23cb0-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="23cb0-140">Incluir propriedades de chave estrangeira em suas entidades (restrições referenciais)</span><span class="sxs-lookup"><span data-stu-id="23cb0-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="23cb0-141">É recomendável que você sempre exponha chaves estrangeiras para relações em suas entidades.</span><span class="sxs-lookup"><span data-stu-id="23cb0-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="23cb0-142">Entity Framework usa uma restrição referencial para identificar que uma propriedade age como a chave estrangeira para uma relação.</span><span class="sxs-lookup"><span data-stu-id="23cb0-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="23cb0-143">Se você marcou a caixa de seleção ***Adicionar propriedades de chave estrangeira ao nome do tipo de entidade &lt;&gt; entidade*** ao criar uma relação, essa restrição referencial foi adicionada para você.</span><span class="sxs-lookup"><span data-stu-id="23cb0-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="23cb0-144">Quando você usa o designer do EF para adicionar ou editar uma restrição referencial, o designer do EF adiciona ou modifica um elemento **ReferentialConstraint** no conteúdo CSDL do arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="23cb0-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="23cb0-145">Clique duas vezes na associação que você deseja editar.</span><span class="sxs-lookup"><span data-stu-id="23cb0-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="23cb0-146">A caixa de diálogo de **restrição referencial** é exibida.</span><span class="sxs-lookup"><span data-stu-id="23cb0-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="23cb0-147">Na lista suspensa  **principal** , selecione a entidade principal na restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="23cb0-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="23cb0-148">As propriedades de chave da entidade são adicionadas à lista  **chave principal** na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="23cb0-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="23cb0-149">Na lista suspensa  **dependente** , selecione a entidade dependente na restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="23cb0-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="23cb0-150">Para cada chave principal que tenha uma chave dependente, selecione uma chave dependente correspondente nas listas suspensas na coluna **chave dependente** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Restrição REF](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="23cb0-152">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="23cb0-153">Criar e editar mapeamentos de associação</span><span class="sxs-lookup"><span data-stu-id="23cb0-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="23cb0-154">Você pode especificar como uma associação é mapeada para o banco de dados na janela **detalhes do mapeamento** do designer do EF.</span><span class="sxs-lookup"><span data-stu-id="23cb0-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="23cb0-155">Você só pode mapear detalhes para as associações que não têm uma restrição referencial especificada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="23cb0-156">Se uma restrição referencial for especificada, uma propriedade de chave estrangeira será incluída na entidade e você poderá usar os detalhes de mapeamento para a entidade para controlar a qual coluna a chave estrangeira é mapeada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="23cb0-157">Criar um mapeamento de associação</span><span class="sxs-lookup"><span data-stu-id="23cb0-157">Create an association mapping</span></span>

-   <span data-ttu-id="23cb0-158">Clique com o botão direito do mouse em uma associação na superfície de design e selecione **mapeamento de tabela**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="23cb0-159">Isso exibe o mapeamento de associação na janela **detalhes do mapeamento** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="23cb0-160">Clique em **Adicionar uma tabela ou exibição**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="23cb0-161">Uma lista suspensa é exibida, incluindo todas as tabelas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="23cb0-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="23cb0-162">Selecione a tabela na qual a associação será mapeada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="23cb0-163">A janela **detalhes do mapeamento** exibe as extremidades da associação e as propriedades de chave para o tipo de entidade em cada **extremidade**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="23cb0-164">Para cada propriedade de chave, clique na **coluna** campo e selecione a coluna na qual a propriedade será mapeada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Detalhes do mapeamento 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="23cb0-166">Editar um mapeamento de associação</span><span class="sxs-lookup"><span data-stu-id="23cb0-166">Edit an association mapping</span></span>

-   <span data-ttu-id="23cb0-167">Clique com o botão direito do mouse em uma associação na superfície de design e selecione **mapeamento de tabela**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="23cb0-168">Isso exibe o mapeamento de associação na janela **detalhes do mapeamento** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="23cb0-169">Clique em **mapas para &lt;nome da tabela&gt;** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="23cb0-170">Uma lista suspensa é exibida, incluindo todas as tabelas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="23cb0-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="23cb0-171">Selecione a tabela na qual a associação será mapeada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="23cb0-172">A janela **detalhes do mapeamento** exibe as extremidades da associação e as propriedades de chave para o tipo de entidade em cada extremidade.</span><span class="sxs-lookup"><span data-stu-id="23cb0-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="23cb0-173">Para cada propriedade de chave, clique na **coluna** campo e selecione a coluna na qual a propriedade será mapeada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="23cb0-174">Editar e excluir propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="23cb0-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="23cb0-175">As propriedades de navegação são propriedades de atalho que são usadas para localizar as entidades nas extremidades de uma associação em um modelo.</span><span class="sxs-lookup"><span data-stu-id="23cb0-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="23cb0-176">As propriedades de navegação podem ser criadas quando você cria uma associação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="23cb0-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="23cb0-177">Para editar propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="23cb0-177">To edit navigation properties</span></span>

-   <span data-ttu-id="23cb0-178">Selecione uma propriedade de navegação na superfície do designer do EF.</span><span class="sxs-lookup"><span data-stu-id="23cb0-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="23cb0-179">As informações sobre a propriedade de navegação são exibidas na janela de de **Propriedades** do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23cb0-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="23cb0-180">Altere as configurações de propriedade na janela **propriedades** .</span><span class="sxs-lookup"><span data-stu-id="23cb0-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="23cb0-181">Para excluir propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="23cb0-181">To delete navigation properties</span></span>

-   <span data-ttu-id="23cb0-182">Se as chaves estrangeiras não forem expostas em tipos de entidade no modelo conceitual, a exclusão de uma propriedade de navegação poderá tornar a passagem de associação correspondente em apenas uma direção ou não pode ser atravessada.</span><span class="sxs-lookup"><span data-stu-id="23cb0-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="23cb0-183">Clique com o botão direito do mouse em uma propriedade de navegação na superfície do designer do EF e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="23cb0-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
