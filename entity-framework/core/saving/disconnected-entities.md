---
title: Entidades Desconectadas – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152410"
---
# <a name="disconnected-entities"></a><span data-ttu-id="28029-102">Entidades desconectadas</span><span class="sxs-lookup"><span data-stu-id="28029-102">Disconnected entities</span></span>

<span data-ttu-id="28029-103">Uma instância DbContext automaticamente controlará as entidades retornadas do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28029-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="28029-104">As alterações feitas a essas entidades serão detectadas quando SaveChanges for chamado e o banco de dados será atualizado conforme o necessário.</span><span class="sxs-lookup"><span data-stu-id="28029-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="28029-105">Veja [Salvamento Básico](basic.md) e [Dados Relacionados](related-data.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="28029-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="28029-106">No entanto, às vezes, as entidades são consultadas usando uma instância de contexto e, em seguida, salvas usando uma instância diferente.</span><span class="sxs-lookup"><span data-stu-id="28029-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="28029-107">Isso geralmente ocorre em cenários "desconectados", por exemplo, um aplicativo da Web onde as entidades são consultadas, enviadas ao cliente, modificadas, enviadas de volta para o servidor em uma solicitação e salvas.</span><span class="sxs-lookup"><span data-stu-id="28029-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="28029-108">Nesse caso, o contexto da segunda instância precisa saber se as entidades são novas (devem ser inseridas) ou existentes (devem ser atualizadas).</span><span class="sxs-lookup"><span data-stu-id="28029-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="28029-109">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="28029-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="28029-110">O EF Core só pode controlar uma instância de qualquer entidade com um determinado valor de chave primária.</span><span class="sxs-lookup"><span data-stu-id="28029-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="28029-111">A melhor maneira de evitar que esse seja um problema é usar um contexto de curta duração para cada unidade de trabalho, de modo que o contexto começa vazio, tem entidades anexadas a ele, salva essas entidades e, em seguida, o contexto é descartado.</span><span class="sxs-lookup"><span data-stu-id="28029-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="28029-112">Identificando novas entidades</span><span class="sxs-lookup"><span data-stu-id="28029-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="28029-113">O cliente identifica novas entidades</span><span class="sxs-lookup"><span data-stu-id="28029-113">Client identifies new entities</span></span>

<span data-ttu-id="28029-114">O caso mais simples para lidar com isso é quando o cliente informa ao servidor se a entidade é nova ou existente.</span><span class="sxs-lookup"><span data-stu-id="28029-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="28029-115">Por exemplo, geralmente, a solicitação para inserir uma nova entidade é diferente da solicitação para atualizar uma entidade existente.</span><span class="sxs-lookup"><span data-stu-id="28029-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="28029-116">O restante desta seção aborda os casos onde é necessário determinar se deseja inserir ou atualizar de alguma outra maneira.</span><span class="sxs-lookup"><span data-stu-id="28029-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="28029-117">Com as chaves geradas automaticamente</span><span class="sxs-lookup"><span data-stu-id="28029-117">With auto-generated keys</span></span>

<span data-ttu-id="28029-118">O valor de uma chave gerada automaticamente geralmente pode ser usado para determinar se uma entidade precisa ser inserida ou atualizada.</span><span class="sxs-lookup"><span data-stu-id="28029-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="28029-119">Se a chave não tiver sido definida (ou seja, ela ainda tem o valor padrão CLR de nulo, zero etc.), a entidade deverá ser nova e precisa de inserção.</span><span class="sxs-lookup"><span data-stu-id="28029-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="28029-120">Por outro lado, se o valor da chave tiver sido definido, ele já deverá ter sido salvo anteriormente e agora precisa ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="28029-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="28029-121">Em outras palavras, se a chave tem um valor, a entidade foi consultada, enviada para o cliente e agora volta para ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="28029-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="28029-122">É fácil verificar se há uma chave não definida quando o tipo de entidade é desconhecido:</span><span class="sxs-lookup"><span data-stu-id="28029-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="28029-123">No entanto, o EF também tem uma forma interna de fazer isso para qualquer tipo de entidade e o tipo de chave:</span><span class="sxs-lookup"><span data-stu-id="28029-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="28029-124">As chaves são definidas assim que as entidades são controladas pelo contexto, mesmo se a entidade estiver no estado adicionado.</span><span class="sxs-lookup"><span data-stu-id="28029-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="28029-125">Isso ajuda ao passar um gráfico de entidades e decidir o que fazer com cada, por exemplo, ao usar a API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="28029-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="28029-126">O valor da chave somente deve ser usado da forma mostrada aqui _antes_ que alguma chamada seja feita para controlar a entidade.</span><span class="sxs-lookup"><span data-stu-id="28029-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="28029-127">Com outras chaves</span><span class="sxs-lookup"><span data-stu-id="28029-127">With other keys</span></span>

<span data-ttu-id="28029-128">Algum outro mecanismo é necessário para identificar novas entidades quando os valores de chave não são gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="28029-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="28029-129">Há duas abordagens gerais para isso:</span><span class="sxs-lookup"><span data-stu-id="28029-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="28029-130">Consulta para a entidade</span><span class="sxs-lookup"><span data-stu-id="28029-130">Query for the entity</span></span>
 * <span data-ttu-id="28029-131">Passar um sinalizador do cliente</span><span class="sxs-lookup"><span data-stu-id="28029-131">Pass a flag from the client</span></span>

<span data-ttu-id="28029-132">Para consultar para a entidade, use apenas o método de localização:</span><span class="sxs-lookup"><span data-stu-id="28029-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="28029-133">Está além do escopo deste documento mostrar o código completo para passar um sinalizador de um cliente.</span><span class="sxs-lookup"><span data-stu-id="28029-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="28029-134">Em um aplicativo Web, isso geralmente significa fazer solicitações diferentes para diferentes ações, ou passar algum estado na solicitação e, em seguida, extraí-lo no controlador.</span><span class="sxs-lookup"><span data-stu-id="28029-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="28029-135">Como salvar entidades simples</span><span class="sxs-lookup"><span data-stu-id="28029-135">Saving single entities</span></span>

<span data-ttu-id="28029-136">Se ele for conhecido ou não, uma inserção ou atualização será necessária, em seguida, adicionar ou atualizar pode ser usado de forma apropriada:</span><span class="sxs-lookup"><span data-stu-id="28029-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="28029-137">No entanto, se a entidade usar valores de chave gerada automaticamente, o método de atualização poderá ser usado para ambos os casos:</span><span class="sxs-lookup"><span data-stu-id="28029-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="28029-138">O método de atualização normalmente marca a entidade para a atualização, não inserção.</span><span class="sxs-lookup"><span data-stu-id="28029-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="28029-139">No entanto, se a entidade tem uma chave gerada automaticamente e nenhum valor de chave tiver sido definido, a entidade será automaticamente marcada para inserir.</span><span class="sxs-lookup"><span data-stu-id="28029-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="28029-140">Esse comportamento foi introduzido no EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="28029-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="28029-141">Para versões anteriores, sempre é necessário escolher explicitamente adicionar ou atualizar.</span><span class="sxs-lookup"><span data-stu-id="28029-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="28029-142">Se a entidade não estiver usando as chaves geradas automaticamente, o aplicativo deverá decidir se a entidade deve ser inserida ou atualizada. Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="28029-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="28029-143">As etapas aqui são:</span><span class="sxs-lookup"><span data-stu-id="28029-143">The steps here are:</span></span>
* <span data-ttu-id="28029-144">Se Localizar retornar nulo, isso significa que o banco de dados ainda não contém o blog com essa ID; portanto, chamamos Adicionar e a marcaremos para inserção.</span><span class="sxs-lookup"><span data-stu-id="28029-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="28029-145">Se Localizar retornar uma entidade, ela existirá no banco de dados e o contexto agora é controlar a entidade existente</span><span class="sxs-lookup"><span data-stu-id="28029-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="28029-146">Em seguida, usamos SetValues para definir os valores de todas as propriedades nessa entidade para os estados que vieram do cliente.</span><span class="sxs-lookup"><span data-stu-id="28029-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="28029-147">A chamada SetValues marcará a entidade para ser atualizada conforme o necessário.</span><span class="sxs-lookup"><span data-stu-id="28029-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="28029-148">SetValues somente marcará como modificadas as propriedades que têm valores diferentes para aqueles na entidade controlada.</span><span class="sxs-lookup"><span data-stu-id="28029-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="28029-149">Isso significa que, quando a atualização é enviada, somente as colunas que realmente foram alteradas serão atualizadas.</span><span class="sxs-lookup"><span data-stu-id="28029-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="28029-150">(E, se nada foi alterado, nenhuma atualização será enviada).</span><span class="sxs-lookup"><span data-stu-id="28029-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="28029-151">Como trabalhar com gráficos</span><span class="sxs-lookup"><span data-stu-id="28029-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="28029-152">Resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="28029-152">Identity resolution</span></span>

<span data-ttu-id="28029-153">Conforme observado acima, o EF Core só pode controlar uma instância de qualquer entidade com um determinado valor de chave primária.</span><span class="sxs-lookup"><span data-stu-id="28029-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="28029-154">Ao trabalhar com elementos gráficos, o gráfico deve ser criado idealmente de modo que essa constante seja mantida e o contexto deve ser usado para apenas uma unidade de trabalho.</span><span class="sxs-lookup"><span data-stu-id="28029-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="28029-155">Se o gráfico contiver duplicatas, será necessário processar o gráfico antes de enviá-lo ao EF para consolidar várias instâncias em uma.</span><span class="sxs-lookup"><span data-stu-id="28029-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="28029-156">Isso pode não ser trivial onde as instâncias têm valores e relações conflitantes, de modo que consolidar duplicatas deve ser feito assim que possível em seu pipeline de aplicativo para evitar a resolução de conflitos.</span><span class="sxs-lookup"><span data-stu-id="28029-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="28029-157">Todas as entidades novas ou existentes</span><span class="sxs-lookup"><span data-stu-id="28029-157">All new/all existing entities</span></span>

<span data-ttu-id="28029-158">Um exemplo de como trabalhar com elementos gráficos é inserir ou atualizar um blog junto com sua coleção de postagens associadas.</span><span class="sxs-lookup"><span data-stu-id="28029-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="28029-159">Se todas as entidades no gráfico tiverem que ser inseridas, ou todas tiverem que ser atualizadas, o processo será o mesmo descrito acima para entidades únicas.</span><span class="sxs-lookup"><span data-stu-id="28029-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="28029-160">Por exemplo, um gráfico de blogs e postagens criados desta forma:</span><span class="sxs-lookup"><span data-stu-id="28029-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="28029-161">pode ser inserido assim:</span><span class="sxs-lookup"><span data-stu-id="28029-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="28029-162">A chamada para adicionar marcará o blog e todas as postagens a serem inseridas.</span><span class="sxs-lookup"><span data-stu-id="28029-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="28029-163">Da mesma forma, se todas as entidades em um gráfico precisarem ser atualizados, atualização pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="28029-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="28029-164">O blog e todas as suas postagens serão marcados para serem atualizados.</span><span class="sxs-lookup"><span data-stu-id="28029-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="28029-165">Combinação de entidades novas e existentes</span><span class="sxs-lookup"><span data-stu-id="28029-165">Mix of new and existing entities</span></span>

<span data-ttu-id="28029-166">Com as chaves geradas automaticamente, a atualização pode novamente ser usada para inserções e atualizações, mesmo se o gráfico contiver uma mistura de entidades que exigem inserção e as que precisam de atualização:</span><span class="sxs-lookup"><span data-stu-id="28029-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="28029-167">A atualização marcará qualquer entidade no gráfico, blog ou postagem para inserção se não tiver um conjunto de valores de chave, enquanto todas as outras entidades estejam marcadas para atualização.</span><span class="sxs-lookup"><span data-stu-id="28029-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="28029-168">Como antes, quando não estiver usando as chaves geradas automaticamente, uma consulta e algum processamento poderão ser usados:</span><span class="sxs-lookup"><span data-stu-id="28029-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="28029-169">Tratamento de exclusões</span><span class="sxs-lookup"><span data-stu-id="28029-169">Handling deletes</span></span>

<span data-ttu-id="28029-170">A exclusão pode ser complicada de lidar porque geralmente a ausência de uma entidade significa que ela deve ser excluída.</span><span class="sxs-lookup"><span data-stu-id="28029-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="28029-171">Uma maneira de lidar com isso é usar "exclusões a quente", de modo que a entidade seja marcada como excluída, em vez de ser excluída de fato.</span><span class="sxs-lookup"><span data-stu-id="28029-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="28029-172">Exclui e, em seguida, torna-se o mesmo que as atualizações.</span><span class="sxs-lookup"><span data-stu-id="28029-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="28029-173">As exclusões a quente podem ser implementadas usando [filtros de consulta](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="28029-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="28029-174">Para exclusões verdadeiras, um padrão comum é usar uma extensão do padrão de consulta para executar o que é essencialmente uma diferença de gráfico. Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="28029-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="28029-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="28029-175">TrackGraph</span></span>

<span data-ttu-id="28029-176">Internamente, adicionar, anexar e atualizar usam percurso de gráfico com uma determinação feita para cada entidade como se ela deve ser marcada como adicionada (para inserir), modificada (para atualizar), inalterada (não fazer nada) ou excluída (para excluir).</span><span class="sxs-lookup"><span data-stu-id="28029-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="28029-177">Esse mecanismo é exposto por meio da API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="28029-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="28029-178">Por exemplo, vamos supor que, quando o cliente envia de volta um gráfico de entidades, ele define alguns sinalizadores em cada entidade indicando como ela deve ser tratada.</span><span class="sxs-lookup"><span data-stu-id="28029-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="28029-179">O TrackGraph pode ser usado para processar esse sinalizador:</span><span class="sxs-lookup"><span data-stu-id="28029-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="28029-180">Os sinalizadores são mostrados apenas como parte da entidade para manter a simplicidade do exemplo.</span><span class="sxs-lookup"><span data-stu-id="28029-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="28029-181">Normalmente, os sinalizadores devem fazer parte de um DTO ou algum outro estado incluído na solicitação.</span><span class="sxs-lookup"><span data-stu-id="28029-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
