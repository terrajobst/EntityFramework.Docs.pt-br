---
title: Entidades desconectadas - Core EF
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="b4e9f-102">Entidades desconectadas</span><span class="sxs-lookup"><span data-stu-id="b4e9f-102">Disconnected entities</span></span>

<span data-ttu-id="b4e9f-103">Uma instância de DbContext automaticamente controlará entidades retornadas do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="b4e9f-104">As alterações feitas a essas entidades, em seguida, serão detectadas quando SaveChanges é chamado e o banco de dados será atualizado conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="b4e9f-105">Consulte [salvar básico](basic.md) e [dados relacionados](related-data.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="b4e9f-106">No entanto, às vezes, entidades são consultadas usando uma instância de contexto e, em seguida, salva usando uma instância diferente.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="b4e9f-107">Isso geralmente ocorre em cenários "desconectados" como um aplicativo da web onde as entidades são consultadas, enviadas ao cliente, modificadas, enviadas de volta para o servidor em uma solicitação e salvos.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="b4e9f-108">Nesse caso, o contexto da segunda instância necessidades saber se as entidades são novo (deve ser inserido) ou existente (deve ser atualizado).</span><span class="sxs-lookup"><span data-stu-id="b4e9f-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="b4e9f-109">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="b4e9f-110">Identificar novas entidades</span><span class="sxs-lookup"><span data-stu-id="b4e9f-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="b4e9f-111">Cliente identifica novas entidades</span><span class="sxs-lookup"><span data-stu-id="b4e9f-111">Client identifies new entities</span></span>

<span data-ttu-id="b4e9f-112">O caso mais simples para lidar com é quando o cliente informa ao servidor se a entidade é novo ou existente.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="b4e9f-113">Por exemplo, geralmente, a solicitação para inserir uma nova entidade é diferente da solicitação de atualização de uma entidade existente.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="b4e9f-114">O restante desta seção aborda os casos onde necessário determinar se deseja inserir ou atualizar de alguma outra maneira.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="b4e9f-115">Com as chaves geradas automaticamente</span><span class="sxs-lookup"><span data-stu-id="b4e9f-115">With auto-generated keys</span></span>

<span data-ttu-id="b4e9f-116">O valor de uma chave gerada automaticamente geralmente pode ser usado para determinar se uma entidade precisa ser inserida ou atualizada.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="b4e9f-117">Se a chave não tiver sido definido (ou seja, ele ainda tem o valor padrão CLR de null, zero, etc.), em seguida, a entidade deve ser nova e precisa inserir.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="b4e9f-118">Por outro lado, se o valor da chave tiver sido definido, em seguida, ele deve ter já foi salvo anteriormente e agora precisa ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="b4e9f-119">Em outras palavras, se a chave tem um valor, em seguida, a entidade foi consultada, enviada para o cliente e tem agora volte a ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="b4e9f-120">É fácil verificar se há uma chave não definida quando o tipo de entidade é desconhecido:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="b4e9f-121">No entanto, o EF também tem uma forma interna de fazer isso para qualquer tipo de entidade e o tipo de chave:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="b4e9f-122">As chaves são definidas como entidades são controladas pelo contexto, mesmo se a entidade está em estado adicionado.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="b4e9f-123">Isso ajuda ao passar um gráfico de entidades e decidir o que fazer com cada, tais como ao usar a API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="b4e9f-124">O valor da chave deve ser usado somente da forma mostrada aqui _antes de_ é feita nenhuma chamada para controlar a entidade.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="b4e9f-125">Com outras chaves</span><span class="sxs-lookup"><span data-stu-id="b4e9f-125">With other keys</span></span>

<span data-ttu-id="b4e9f-126">Outro mecanismo é necessário para novas entidades de identidade quando os valores de chave não são gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-126">Some other mechanism is needed to identity new entities when key values are not generated automatically.</span></span> <span data-ttu-id="b4e9f-127">Há duas abordagens gerais para isso:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="b4e9f-128">Consulta para a entidade</span><span class="sxs-lookup"><span data-stu-id="b4e9f-128">Query for the entity</span></span>
 * <span data-ttu-id="b4e9f-129">Passar um sinalizador do cliente</span><span class="sxs-lookup"><span data-stu-id="b4e9f-129">Pass a flag from the client</span></span>

<span data-ttu-id="b4e9f-130">Para consultar para a entidade, use apenas o método de localização:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="b4e9f-131">Está além do escopo deste documento para mostrar o código completo para transmitir um sinalizador de um cliente.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="b4e9f-132">Em um aplicativo web, geralmente isso significa fazer solicitações diferentes para diferentes ações, ou passando algum estado na solicitação, extraindo-o no controlador.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="b4e9f-133">Salvando entidades simples</span><span class="sxs-lookup"><span data-stu-id="b4e9f-133">Saving single entities</span></span>

<span data-ttu-id="b4e9f-134">Se ele for conhecido ou não uma inserção ou atualização é necessária, em seguida, adicionar ou atualizar pode ser usado de forma apropriada:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="b4e9f-135">No entanto, se a entidade usa valores de chave gerada automaticamente, o método de atualização pode ser usado para ambos os casos:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="b4e9f-136">O método de atualização normalmente marca da entidade para a atualização, inserção não.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="b4e9f-137">No entanto, se a entidade tem uma chave gerada automaticamente e nenhum valor de chave foi definida, em seguida, em vez disso, a entidade é automaticamente marcada para inserir.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="b4e9f-138">Esse comportamento foi introduzido no EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="b4e9f-139">Para versões anteriores sempre é necessário escolher explicitamente adicionar ou atualizar.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="b4e9f-140">Se a entidade não está usando as chaves geradas automaticamente, o aplicativo deve decidir se a entidade deve ser inserida ou atualizada: por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="b4e9f-141">As etapas aqui são:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-141">The steps here are:</span></span>
* <span data-ttu-id="b4e9f-142">Se adicionar localizar retorna null, e o banco de dados ainda não contiver o blog com essa ID, portanto, podemos chamar marcá-la para inserção.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="b4e9f-143">Se encontrar retorna uma entidade, em seguida, ele existe no banco de dados e o contexto agora está controlando a entidade existente</span><span class="sxs-lookup"><span data-stu-id="b4e9f-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="b4e9f-144">Em seguida, usamos o SetValues para definir os valores de todas as propriedades nessa entidade para os estados do cliente.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="b4e9f-145">A chamada SetValues marcará a entidade a ser atualizada conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="b4e9f-146">SetValues somente marcará como modificou as propriedades que têm valores diferentes para aqueles na entidade controlada.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="b4e9f-147">Isso significa que, quando a atualização é enviada, somente as colunas que realmente foram alterados sejam atualizadas.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="b4e9f-148">(E se nada foi alterado, em seguida, nenhuma atualização será enviada em todos os).</span><span class="sxs-lookup"><span data-stu-id="b4e9f-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="b4e9f-149">Trabalhando com gráficos</span><span class="sxs-lookup"><span data-stu-id="b4e9f-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="b4e9f-150">Todas as entidades existentes do novo/all</span><span class="sxs-lookup"><span data-stu-id="b4e9f-150">All new/all existing entities</span></span>

<span data-ttu-id="b4e9f-151">Um exemplo de como trabalhar com elementos gráficos é inserir ou atualizar um blog junto com sua coleção de postagens associadas.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="b4e9f-152">Se todas as entidades no gráfico devem ser inseridas, ou todos devem ser atualizados, o processo é o mesmo descrito acima para entidades únicas.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="b4e9f-153">Por exemplo, um gráfico de blogs e postagens criadas desta forma:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="b4e9f-154">pode ser inserido como este:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="b4e9f-155">A chamada para adicionar marcará o blog e postagens a ser inserido.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="b4e9f-156">Da mesma forma, se todas as entidades em um gráfico que precisam ser atualizados, em seguida, atualização pode ser usada:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="b4e9f-157">O blog e todas as suas mensagens serão marcadas para serem atualizados.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="b4e9f-158">Combinação de entidades novas e existentes</span><span class="sxs-lookup"><span data-stu-id="b4e9f-158">Mix of new and existing entities</span></span>

<span data-ttu-id="b4e9f-159">Com as chaves geradas automaticamente, atualização pode novamente ser usada para inserções e atualizações, mesmo que o gráfico contém uma mistura de entidades que exigem inserção e aqueles que precisam de atualização:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="b4e9f-160">Atualização marcará a qualquer entidade no gráfico, blog ou post para inserção se não tiver um conjunto de valores de chave, enquanto todas as outras entidades são marcadas para atualização.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="b4e9f-161">Como antes, quando não estiver usando as chaves geradas automaticamente, uma consulta e algum processamento podem ser usados:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="b4e9f-162">Tratamento de exclusões</span><span class="sxs-lookup"><span data-stu-id="b4e9f-162">Handling deletes</span></span>

<span data-ttu-id="b4e9f-163">Exclusão pode ser complicada para tratar desde geralmente a ausência de uma entidade significa que deve ser excluído.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="b4e9f-164">Uma maneira de lidar com isso é usar "exclusões a quente", de modo que a entidade está marcada como excluído, em vez de, na verdade, está sendo excluído.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="b4e9f-165">Exclui e torna-se o mesmo que as atualizações.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="b4e9f-166">Exclusões a quente podem ser implementadas usando [filtros de consulta](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="b4e9f-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="b4e9f-167">Para exclusões true, um padrão comum é usar uma extensão do padrão de consulta para executar o que é essencialmente uma diferença de gráfico. Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="b4e9f-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="b4e9f-168">TrackGraph</span></span>

<span data-ttu-id="b4e9f-169">Internamente, adicionar, anexar e atualização usam percurso de gráfico com uma determinação feita para cada entidade como se ele deve ser marcado como adicionado (para inserir), modificado (para atualizar), inalterado (não fazer nada), ou excluídos (para excluir).</span><span class="sxs-lookup"><span data-stu-id="b4e9f-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="b4e9f-170">Esse mecanismo é exposto por meio da API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="b4e9f-171">Por exemplo, vamos supor que, quando o cliente envia de volta um gráfico de entidades ele define algumas sinalizador em cada entidade que indica como devem ser tratada.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="b4e9f-172">TrackGraph pode ser usado para processar esse sinalizador:</span><span class="sxs-lookup"><span data-stu-id="b4e9f-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="b4e9f-173">Os sinalizadores são mostrados apenas como parte da entidade para manter a simplicidade do exemplo.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="b4e9f-174">Normalmente, os sinalizadores seria parte de um DTO ou algum outro estado incluído na solicitação.</span><span class="sxs-lookup"><span data-stu-id="b4e9f-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
