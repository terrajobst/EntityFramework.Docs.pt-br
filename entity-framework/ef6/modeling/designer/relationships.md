---
title: Relações - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: e1912a5e00e51b4f07b1ac83848fdbe0aa4755aa
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250706"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="048eb-102">Relações - EF Designer</span><span class="sxs-lookup"><span data-stu-id="048eb-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="048eb-103">Esta página fornece informações sobre como configurar as relações no seu modelo usando o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="048eb-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="048eb-104">Para obter informações gerais sobre as relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="048eb-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="048eb-105">Associações de definem relações entre os tipos de entidade em um modelo.</span><span class="sxs-lookup"><span data-stu-id="048eb-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="048eb-106">Este tópico mostra como mapear as associações com o Entity Framework Designer (Designer de EF).</span><span class="sxs-lookup"><span data-stu-id="048eb-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="048eb-107">A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="048eb-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="048eb-109">Quando você compila o modelo conceitual, os avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros.</span><span class="sxs-lookup"><span data-stu-id="048eb-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="048eb-110">Você pode ignorar esses avisos porque depois que você optar por gerar o banco de dados do modelo, os erros serão eliminados.</span><span class="sxs-lookup"><span data-stu-id="048eb-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="048eb-111">Visão geral de associações</span><span class="sxs-lookup"><span data-stu-id="048eb-111">Associations Overview</span></span>

<span data-ttu-id="048eb-112">Ao projetar seu modelo usando o EF Designer, um arquivo. edmx representa seu modelo.</span><span class="sxs-lookup"><span data-stu-id="048eb-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="048eb-113">No arquivo. edmx, uma **associação** elemento define uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="048eb-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="048eb-114">Uma associação deve especificar os tipos de entidade envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como a multiplicidade.</span><span class="sxs-lookup"><span data-stu-id="048eb-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="048eb-115">A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (entre 0 e 1) ou muitas (\*).</span><span class="sxs-lookup"><span data-stu-id="048eb-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="048eb-116">Essas informações são especificadas em dois filhos **final** elementos.</span><span class="sxs-lookup"><span data-stu-id="048eb-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="048eb-117">Em tempo de execução, instâncias do tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras (se você optar por expor as chaves estrangeiras nas suas entidades).</span><span class="sxs-lookup"><span data-stu-id="048eb-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="048eb-118">Com chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um **ReferentialConstraint** elemento (um elemento filho do **associação** elemento).</span><span class="sxs-lookup"><span data-stu-id="048eb-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="048eb-119">É recomendável que você exponha sempre chaves estrangeiras para relações nas suas entidades.</span><span class="sxs-lookup"><span data-stu-id="048eb-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="048eb-120">Em muitos-para-muitos (\*:\*) você não pode adicionar chaves estrangeiras para as entidades.</span><span class="sxs-lookup"><span data-stu-id="048eb-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="048eb-121">Em um \*:\* relação, as informações de associação é gerenciada com um objeto independente.</span><span class="sxs-lookup"><span data-stu-id="048eb-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="048eb-122">Para obter informações sobre os elementos CSDL (**ReferentialConstraint**, **associação**, etc.) consulte o [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="048eb-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="048eb-123">Criar e excluir associações</span><span class="sxs-lookup"><span data-stu-id="048eb-123">Create and Delete Associations</span></span>

<span data-ttu-id="048eb-124">Criando uma associação com as atualizações do Designer de EF com o conteúdo do modelo do arquivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="048eb-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="048eb-125">Depois de criar uma associação, você deve criar os mapeamentos para a associação (discutido posteriormente neste tópico).</span><span class="sxs-lookup"><span data-stu-id="048eb-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="048eb-126">Esta seção pressupõe que você já adicionou as entidades que você deseja criar uma associação entre a seu modelo.</span><span class="sxs-lookup"><span data-stu-id="048eb-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="048eb-127">Para criar uma associação</span><span class="sxs-lookup"><span data-stu-id="048eb-127">To create an association</span></span>

1.  <span data-ttu-id="048eb-128">Clique em uma área vazia da superfície de design, aponte para **adicionar novo**e selecione **associação...** .</span><span class="sxs-lookup"><span data-stu-id="048eb-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="048eb-129">Preencha as configurações para a associação na **Adicionar associação** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="048eb-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Adicionar associação](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="048eb-131">Você pode optar por não adicionar propriedades de navegação ou propriedades de chave estrangeira para as entidades nas extremidades da associação, limpando o \* \* propriedade de navegação \* \* e \* \* adicionar propriedades de chave estrangeira para a &lt;nome do tipo de entidade&gt; entidade \* \* caixas de seleção.</span><span class="sxs-lookup"><span data-stu-id="048eb-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the \*\*Navigation Property \*\*and \*\*Add foreign key properties to the &lt;entity type name&gt; Entity \*\*checkboxes.</span></span> <span data-ttu-id="048eb-132">Se você adicionar apenas uma propriedade de navegação, a associação será navegável em uma única direção.</span><span class="sxs-lookup"><span data-stu-id="048eb-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="048eb-133">Se você não adicionar nenhuma propriedade de navegação, você deve optar por adicionar propriedades de chave estrangeira para acessar as entidades nas extremidades da associação.</span><span class="sxs-lookup"><span data-stu-id="048eb-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="048eb-134">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="048eb-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="048eb-135">Para excluir uma associação</span><span class="sxs-lookup"><span data-stu-id="048eb-135">To delete an association</span></span>

<span data-ttu-id="048eb-136">Para excluir uma associação faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="048eb-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="048eb-137">Clique com botão direito a associação no EF Designer na superfície e selecione **excluir**.</span><span class="sxs-lookup"><span data-stu-id="048eb-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="048eb-138">OU-</span><span class="sxs-lookup"><span data-stu-id="048eb-138">OR -</span></span>

-   <span data-ttu-id="048eb-139">Selecione uma ou mais associações e pressione a tecla DELETE.</span><span class="sxs-lookup"><span data-stu-id="048eb-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="048eb-140">Incluir propriedades de chave estrangeira nas suas entidades (restrições referenciais)</span><span class="sxs-lookup"><span data-stu-id="048eb-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="048eb-141">É recomendável que você exponha sempre chaves estrangeiras para relações nas suas entidades.</span><span class="sxs-lookup"><span data-stu-id="048eb-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="048eb-142">Entity Framework usa uma restrição referencial para identificar que uma propriedade atua como a chave estrangeira para uma relação.</span><span class="sxs-lookup"><span data-stu-id="048eb-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="048eb-143">Se você tiver marcado a ***adicionar propriedades de chave estrangeira para a &lt;nome do tipo de entidade&gt; entidade*** caixa de seleção ao criar uma relação, essa restrição referencial foi adicionada para você.</span><span class="sxs-lookup"><span data-stu-id="048eb-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="048eb-144">Quando você usa o EF Designer para adicionar ou editar uma restrição referencial, o EF Designer adiciona ou modifica um **ReferentialConstraint** elemento no conteúdo do arquivo. edmx CSDL.</span><span class="sxs-lookup"><span data-stu-id="048eb-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="048eb-145">Clique duas vezes a associação que você deseja editar.</span><span class="sxs-lookup"><span data-stu-id="048eb-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="048eb-146">O **restrição referencial** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="048eb-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="048eb-147">Dos **Principal** lista suspensa, selecione a entidade principal na restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="048eb-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="048eb-148">Propriedades de chave da entidade são adicionadas para o **chave da entidade** lista na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="048eb-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="048eb-149">Dos **dependentes** lista suspensa, selecione a entidade dependente na restrição referencial.</span><span class="sxs-lookup"><span data-stu-id="048eb-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="048eb-150">Para cada chave de entidade que tem uma chave dependente, selecione uma chave de dependente correspondente nas listas suspensos a **chave dependente** coluna.</span><span class="sxs-lookup"><span data-stu-id="048eb-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Restrição ref](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="048eb-152">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="048eb-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="048eb-153">Criar e editar os mapeamentos de associação</span><span class="sxs-lookup"><span data-stu-id="048eb-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="048eb-154">Você pode especificar como uma associação é mapeada para o banco de dados do **Mapping Details** janela do Designer de EF.</span><span class="sxs-lookup"><span data-stu-id="048eb-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="048eb-155">Você só pode mapear os detalhes para as associações que não têm uma restrição referencial especificada.</span><span class="sxs-lookup"><span data-stu-id="048eb-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="048eb-156">Se uma restrição referencial for especificada, em seguida, uma propriedade de chave estrangeira está incluída na entidade e você pode usar os detalhes de mapeamento para a entidade para controlar qual coluna de chave estrangeira é mapeado para.</span><span class="sxs-lookup"><span data-stu-id="048eb-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="048eb-157">Criar um mapeamento de associação</span><span class="sxs-lookup"><span data-stu-id="048eb-157">Create an association mapping</span></span>

-   <span data-ttu-id="048eb-158">Com uma associação na superfície do design e selecione o botão direito **mapeamento de tabela**.</span><span class="sxs-lookup"><span data-stu-id="048eb-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="048eb-159">Isso exibe o mapeamento de associação na **detalhes de mapeamento** janela.</span><span class="sxs-lookup"><span data-stu-id="048eb-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="048eb-160">Clique em **adicionar uma tabela ou exibição**.</span><span class="sxs-lookup"><span data-stu-id="048eb-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="048eb-161">É exibida uma lista suspensa que inclui todas as tabelas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="048eb-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="048eb-162">Selecione a tabela à qual a associação será mapeado.</span><span class="sxs-lookup"><span data-stu-id="048eb-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="048eb-163">O **Mapping Details** janela exibe ambas as extremidades de associação e as propriedades de chave para o tipo de entidade em cada **final**.</span><span class="sxs-lookup"><span data-stu-id="048eb-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="048eb-164">Para cada propriedade de chave, clique o **coluna** campo e, em seguida, selecione a coluna à qual a propriedade será mapeado.</span><span class="sxs-lookup"><span data-stu-id="048eb-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Detalhes do mapeamento de 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="048eb-166">Editar um mapeamento de associação</span><span class="sxs-lookup"><span data-stu-id="048eb-166">Edit an association mapping</span></span>

-   <span data-ttu-id="048eb-167">Com uma associação na superfície do design e selecione o botão direito **mapeamento de tabela**.</span><span class="sxs-lookup"><span data-stu-id="048eb-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="048eb-168">Isso exibe o mapeamento de associação na **detalhes de mapeamento** janela.</span><span class="sxs-lookup"><span data-stu-id="048eb-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="048eb-169">Clique em **mapeia para &lt;nome da tabela&gt;**.</span><span class="sxs-lookup"><span data-stu-id="048eb-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="048eb-170">É exibida uma lista suspensa que inclui todas as tabelas no modelo de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="048eb-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="048eb-171">Selecione a tabela à qual a associação será mapeado.</span><span class="sxs-lookup"><span data-stu-id="048eb-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="048eb-172">O **Mapping Details** janela exibe ambas as extremidades de associação e as propriedades de chave para o tipo de entidade em cada extremidade.</span><span class="sxs-lookup"><span data-stu-id="048eb-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="048eb-173">Para cada propriedade de chave, clique o **coluna** campo e, em seguida, selecione a coluna à qual a propriedade será mapeado.</span><span class="sxs-lookup"><span data-stu-id="048eb-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="048eb-174">Editar e excluir propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="048eb-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="048eb-175">Propriedades de navegação são propriedades de atalho que são usadas para localizar as entidades nas extremidades de uma associação em um modelo.</span><span class="sxs-lookup"><span data-stu-id="048eb-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="048eb-176">Propriedades de navegação podem ser criadas quando você cria uma associação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="048eb-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="048eb-177">Para editar as propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="048eb-177">To edit navigation properties</span></span>

-   <span data-ttu-id="048eb-178">Selecione uma propriedade de navegação na superfície de Designer de EF.</span><span class="sxs-lookup"><span data-stu-id="048eb-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="048eb-179">Informações sobre a propriedade de navegação são exibidas no Visual Studio **propriedades** janela.</span><span class="sxs-lookup"><span data-stu-id="048eb-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="048eb-180">Alterar as configurações de propriedade na **propriedades** janela.</span><span class="sxs-lookup"><span data-stu-id="048eb-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="048eb-181">Para excluir as propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="048eb-181">To delete navigation properties</span></span>

-   <span data-ttu-id="048eb-182">Se as chaves estrangeiras não são expostas nos tipos de entidade no modelo conceitual, a exclusão de uma propriedade de navegação pode tornar a associação correspondente navegável apenas em uma direção ou não navegável em todos os.</span><span class="sxs-lookup"><span data-stu-id="048eb-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="048eb-183">Clique com botão direito no Designer de EF superfície e selecione uma propriedade de navegação **excluir**.</span><span class="sxs-lookup"><span data-stu-id="048eb-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
