---
title: Colocar em cascata Delete - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a><span data-ttu-id="d7d75-102">Excluir em cascata</span><span class="sxs-lookup"><span data-stu-id="d7d75-102">Cascade Delete</span></span>

<span data-ttu-id="d7d75-103">Exclusão em cascata é geralmente usado na terminologia de banco de dados para descrever uma característica que permite a exclusão de uma linha para disparar automaticamente a exclusão de linhas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d7d75-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="d7d75-104">Um conceito relacionado também coberto por comportamentos de exclusão de EF principal é a exclusão automática de uma entidade filho quando ela é a relação a um pai tem foi desfeita – este i conhecido como "excluir órfãos".</span><span class="sxs-lookup"><span data-stu-id="d7d75-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="d7d75-105">EF Core implementa vários comportamentos de exclusão diferente e permite a configuração dos comportamentos de exclusão de relações individuais.</span><span class="sxs-lookup"><span data-stu-id="d7d75-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="d7d75-106">EF principal também implementa convenções que configuram automaticamente os comportamentos de exclusão padrão útil para cada relação com base em [requiredness da relação] (... /Modeling/Relationships.MD#Required-and-Optional-Relationships).</span><span class="sxs-lookup"><span data-stu-id="d7d75-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="d7d75-107">Excluir comportamentos</span><span class="sxs-lookup"><span data-stu-id="d7d75-107">Delete behaviors</span></span>
<span data-ttu-id="d7d75-108">Excluir comportamentos são definidos no *DeleteBehavior* enumerador de tipo e pode ser passado para o *OnDelete* API fluente para controlar se a exclusão de uma entidade principal/pai ou o corte do relação com entidades dependentes/filho deve ter um efeito colateral nas entidades dependentes/filho.</span><span class="sxs-lookup"><span data-stu-id="d7d75-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="d7d75-109">Há três ações que EF pode ser executadas quando uma entidade principal/pai é excluída ou a relação para o filho é desligada:</span><span class="sxs-lookup"><span data-stu-id="d7d75-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="d7d75-110">O filho/dependente pode ser excluído.</span><span class="sxs-lookup"><span data-stu-id="d7d75-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="d7d75-111">Os valores de chave estrangeira do filho podem ser definidos como null</span><span class="sxs-lookup"><span data-stu-id="d7d75-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="d7d75-112">O filho permanece inalterado</span><span class="sxs-lookup"><span data-stu-id="d7d75-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="d7d75-113">O comportamento de exclusão configurado no modelo de núcleo EF só é aplicado quando a entidade principal é excluída usando EF Core e entidades dependentes são carregadas na memória (ou seja, para controladas dependentes).</span><span class="sxs-lookup"><span data-stu-id="d7d75-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="d7d75-114">Um comportamento em cascata correspondente deve ser a instalação no banco de dados para garantir que não está sendo controlados pelo contexto de dados tem a ação necessária aplicada.</span><span class="sxs-lookup"><span data-stu-id="d7d75-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="d7d75-115">Se você usar o EF principal para criar o banco de dados, esse comportamento em cascata será instalado para você.</span><span class="sxs-lookup"><span data-stu-id="d7d75-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="d7d75-116">Para a segunda ação acima, definir um valor de chave estrangeiro como null não é válido se a chave estrangeira não é anulável.</span><span class="sxs-lookup"><span data-stu-id="d7d75-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="d7d75-117">(Uma chave estrangeira não anuláveis é equivalente a uma relação necessária.) Nesses casos, Core EF controla se a propriedade de chave estrangeira foi marcada como nulo até SaveChanges é chamado, o momento em que uma exceção será lançada porque as alterações não podem ser mantidas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d7d75-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="d7d75-118">Isso é semelhante à obtenção de uma violação de restrição do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d7d75-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="d7d75-119">Existem em quatro excluir comportamentos, conforme listado nas tabelas a seguir.</span><span class="sxs-lookup"><span data-stu-id="d7d75-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="d7d75-120">Para relações opcionais (chave estrangeira anulável)- _é_ possível salvar um nulo valor de chave estrangeiro, que resulta nos seguintes efeitos:</span><span class="sxs-lookup"><span data-stu-id="d7d75-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="d7d75-121">Nome de comportamento</span><span class="sxs-lookup"><span data-stu-id="d7d75-121">Behavior Name</span></span> | <span data-ttu-id="d7d75-122">Efeito em dependente/filho na memória</span><span class="sxs-lookup"><span data-stu-id="d7d75-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="d7d75-123">Efeito em dependente/filho no banco de dados</span><span class="sxs-lookup"><span data-stu-id="d7d75-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="d7d75-124">**Em cascata**</span><span class="sxs-lookup"><span data-stu-id="d7d75-124">**Cascade**</span></span> | <span data-ttu-id="d7d75-125">Entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="d7d75-125">Entities are deleted</span></span> | <span data-ttu-id="d7d75-126">Entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="d7d75-126">Entities are deleted</span></span>
| <span data-ttu-id="d7d75-127">**ClientSetNull** (padrão)</span><span class="sxs-lookup"><span data-stu-id="d7d75-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="d7d75-128">Propriedades de chave estrangeira são definidas como null</span><span class="sxs-lookup"><span data-stu-id="d7d75-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="d7d75-129">Nenhum</span><span class="sxs-lookup"><span data-stu-id="d7d75-129">None</span></span>
| <span data-ttu-id="d7d75-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="d7d75-130">**SetNull**</span></span> | <span data-ttu-id="d7d75-131">Propriedades de chave estrangeira são definidas como null</span><span class="sxs-lookup"><span data-stu-id="d7d75-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="d7d75-132">Propriedades de chave estrangeira são definidas como null</span><span class="sxs-lookup"><span data-stu-id="d7d75-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="d7d75-133">**Restringir**</span><span class="sxs-lookup"><span data-stu-id="d7d75-133">**Restrict**</span></span> | <span data-ttu-id="d7d75-134">Nenhum</span><span class="sxs-lookup"><span data-stu-id="d7d75-134">None</span></span> | <span data-ttu-id="d7d75-135">Nenhum</span><span class="sxs-lookup"><span data-stu-id="d7d75-135">None</span></span>

<span data-ttu-id="d7d75-136">Para relações necessárias (chave estrangeira não anuláveis) é _não_ possível salvar um nulo valor de chave estrangeiro, que resulta nos seguintes efeitos:</span><span class="sxs-lookup"><span data-stu-id="d7d75-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="d7d75-137">Nome de comportamento</span><span class="sxs-lookup"><span data-stu-id="d7d75-137">Behavior Name</span></span> | <span data-ttu-id="d7d75-138">Efeito em dependente/filho na memória</span><span class="sxs-lookup"><span data-stu-id="d7d75-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="d7d75-139">Efeito em dependente/filho no banco de dados</span><span class="sxs-lookup"><span data-stu-id="d7d75-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="d7d75-140">**CASCADE** (padrão)</span><span class="sxs-lookup"><span data-stu-id="d7d75-140">**Cascade** (Default)</span></span> | <span data-ttu-id="d7d75-141">Entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="d7d75-141">Entities are deleted</span></span> | <span data-ttu-id="d7d75-142">Entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="d7d75-142">Entities are deleted</span></span>
| <span data-ttu-id="d7d75-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="d7d75-143">**ClientSetNull**</span></span> | <span data-ttu-id="d7d75-144">SaveChanges lança</span><span class="sxs-lookup"><span data-stu-id="d7d75-144">SaveChanges throws</span></span> | <span data-ttu-id="d7d75-145">Nenhum</span><span class="sxs-lookup"><span data-stu-id="d7d75-145">None</span></span>
| <span data-ttu-id="d7d75-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="d7d75-146">**SetNull**</span></span> | <span data-ttu-id="d7d75-147">SaveChanges lança</span><span class="sxs-lookup"><span data-stu-id="d7d75-147">SaveChanges throws</span></span> | <span data-ttu-id="d7d75-148">SaveChanges lança</span><span class="sxs-lookup"><span data-stu-id="d7d75-148">SaveChanges throws</span></span>
| <span data-ttu-id="d7d75-149">**Restringir**</span><span class="sxs-lookup"><span data-stu-id="d7d75-149">**Restrict**</span></span> | <span data-ttu-id="d7d75-150">Nenhum</span><span class="sxs-lookup"><span data-stu-id="d7d75-150">None</span></span> | <span data-ttu-id="d7d75-151">Nenhum</span><span class="sxs-lookup"><span data-stu-id="d7d75-151">None</span></span>

<span data-ttu-id="d7d75-152">Nas tabelas acima, *nenhum* pode resultar em uma violação de restrição.</span><span class="sxs-lookup"><span data-stu-id="d7d75-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="d7d75-153">Por exemplo, se uma entidade principal/filho é excluída, mas nenhuma ação será tomada para alterar a chave estrangeira da dependente/filho, em seguida, o banco de dados provavelmente lançará em SaveChanges devido a uma violação de restrição foreign.</span><span class="sxs-lookup"><span data-stu-id="d7d75-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="d7d75-154">Em um alto nível:</span><span class="sxs-lookup"><span data-stu-id="d7d75-154">At a high level:</span></span>
* <span data-ttu-id="d7d75-155">Se você tiver entidades que não podem existir sem um pai, e você deseja EF para cuidar para excluir os filhos automaticamente e usa *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="d7d75-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="d7d75-156">Entidades que não podem existir sem um pai geralmente faz uso de relações necessárias, para o qual *Cascade* é o padrão.</span><span class="sxs-lookup"><span data-stu-id="d7d75-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="d7d75-157">Se você tiver entidades que podem ou não ter um pai, e você deseja EF para cuidar de anular a chave estrangeira para você, e depois usar *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="d7d75-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="d7d75-158">Entidades que podem existir sem um pai geralmente faz uso de relações opcionais, para o qual *ClientSetNull* é o padrão.</span><span class="sxs-lookup"><span data-stu-id="d7d75-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="d7d75-159">Se você quiser que o banco de dados também tenta propagar os valores nulos para chaves estrangeiras filho até mesmo quando a entidade filho não está carregada, em seguida, use *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="d7d75-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="d7d75-160">No entanto, observe que o banco de dados deve oferecer suporte a isso e configurar o banco de dados como isso pode resultar em outras restrições, que, na prática, geralmente faz essa opção impraticável.</span><span class="sxs-lookup"><span data-stu-id="d7d75-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="d7d75-161">É por isso que *SetNull* não é o padrão.</span><span class="sxs-lookup"><span data-stu-id="d7d75-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="d7d75-162">Se você não quiser núcleo para nunca excluir uma entidade automaticamente ou null automaticamente a chave estrangeira, em seguida, usar o EF *restringir*.</span><span class="sxs-lookup"><span data-stu-id="d7d75-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="d7d75-163">Observe que isso requer que seu código manter entidades filho e seus valores de chave estrangeiras em sincronia manualmente caso contrário, as exceções de restrição serão lançadas.</span><span class="sxs-lookup"><span data-stu-id="d7d75-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="d7d75-164">No núcleo do EF, ao contrário de EF6, efeitos em cascata não ocorrer imediatamente, mas em vez disso, apenas quando SaveChanges é chamado.</span><span class="sxs-lookup"><span data-stu-id="d7d75-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="d7d75-165">**Alterações no EF Core 2.0:** em versões anteriores, *restringir* causaria opcionais propriedades de chave estrangeira em entidades dependentes controladas para ser definido como null e era o padrão excluir comportamento para relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="d7d75-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="d7d75-166">No EF Core 2.0, o *ClientSetNull* foi introduzido para representar esse comportamento e tornou-se o padrão de relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="d7d75-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="d7d75-167">O comportamento de *restringir* foi ajustado para nunca têm efeitos colaterais em entidades dependentes.</span><span class="sxs-lookup"><span data-stu-id="d7d75-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="d7d75-168">Exemplos de exclusão de entidade</span><span class="sxs-lookup"><span data-stu-id="d7d75-168">Entity deletion examples</span></span>

<span data-ttu-id="d7d75-169">O código a seguir faz parte de um [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que pode ser baixado um tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="d7d75-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="d7d75-170">O exemplo mostra o que acontece para cada comportamento de exclusão de relações necessários e opcionais quando uma entidade pai é excluída.</span><span class="sxs-lookup"><span data-stu-id="d7d75-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="d7d75-171">Vamos examinar cada variação para entender o que está acontecendo.</span><span class="sxs-lookup"><span data-stu-id="d7d75-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="d7d75-172">DeleteBehavior.Cascade com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="d7d75-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="d7d75-173">Blog está marcado como excluído</span><span class="sxs-lookup"><span data-stu-id="d7d75-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="d7d75-174">Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="d7d75-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="d7d75-175">SaveChanges envia exclusões para dependentes/filhos (postagens) e, em seguida, o principal/pai (blog)</span><span class="sxs-lookup"><span data-stu-id="d7d75-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="d7d75-176">Depois de salvar, todas as entidades são desanexadas desde agora foram excluídas do banco de dados</span><span class="sxs-lookup"><span data-stu-id="d7d75-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="d7d75-177">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação necessária</span><span class="sxs-lookup"><span data-stu-id="d7d75-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="d7d75-178">Blog está marcado como excluído</span><span class="sxs-lookup"><span data-stu-id="d7d75-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="d7d75-179">Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="d7d75-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="d7d75-180">SaveChanges tenta definir a postagem FK como nulo, mas falha porque a CE não é anulável</span><span class="sxs-lookup"><span data-stu-id="d7d75-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="d7d75-181">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional</span><span class="sxs-lookup"><span data-stu-id="d7d75-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="d7d75-182">Blog está marcado como excluído</span><span class="sxs-lookup"><span data-stu-id="d7d75-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="d7d75-183">Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="d7d75-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="d7d75-184">Tentativas de SaveChanges define FK (postagens) em filhos dependentes/como nulo antes de excluir a entidade de segurança/pai (blog)</span><span class="sxs-lookup"><span data-stu-id="d7d75-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="d7d75-185">Depois de salvar, o principal/pai (blog) será excluído, mas os seus dependentes/filhos (postagens) ainda são controlados</span><span class="sxs-lookup"><span data-stu-id="d7d75-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="d7d75-186">Os dependentes/filhos controlados (postagens) agora tem valores nulos de FK e sua referência para a entidade de segurança/pai excluído (blog) foi removida</span><span class="sxs-lookup"><span data-stu-id="d7d75-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="d7d75-187">DeleteBehavior.Restrict com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="d7d75-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="d7d75-188">Blog está marcado como excluído</span><span class="sxs-lookup"><span data-stu-id="d7d75-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="d7d75-189">Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="d7d75-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="d7d75-190">Como *restringir* informa ao EF para definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges gera sem salvar</span><span class="sxs-lookup"><span data-stu-id="d7d75-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="d7d75-191">Exemplos de órfãos de exclusão</span><span class="sxs-lookup"><span data-stu-id="d7d75-191">Delete orphans examples</span></span>

<span data-ttu-id="d7d75-192">O código a seguir faz parte de um [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que pode ser baixado um tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="d7d75-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="d7d75-193">O exemplo mostra o que acontece para cada comportamento de exclusão de relações necessários e opcionais quando a relação entre um pai/principal e seus filhos/dependentes for desfeita.</span><span class="sxs-lookup"><span data-stu-id="d7d75-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="d7d75-194">Neste exemplo, a relação for desfeita, removendo os dependentes/filhos (postagens) da propriedade de navegação da coleção no principal/pai (blog).</span><span class="sxs-lookup"><span data-stu-id="d7d75-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="d7d75-195">No entanto, o comportamento é o mesmo se a referência de dependente/filho ao principal/pai em vez disso, é nulled-out.</span><span class="sxs-lookup"><span data-stu-id="d7d75-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="d7d75-196">Vamos examinar cada variação para entender o que está acontecendo.</span><span class="sxs-lookup"><span data-stu-id="d7d75-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="d7d75-197">DeleteBehavior.Cascade com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="d7d75-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="d7d75-198">Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="d7d75-199">Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="d7d75-200">SaveChanges envia exclusões para dependentes/filhos (postagens)</span><span class="sxs-lookup"><span data-stu-id="d7d75-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="d7d75-201">Depois de salvar, dependentes/filhos (postagens) são desanexados desde agora foram excluídas do banco de dados</span><span class="sxs-lookup"><span data-stu-id="d7d75-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="d7d75-202">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação necessária</span><span class="sxs-lookup"><span data-stu-id="d7d75-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="d7d75-203">Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="d7d75-204">Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="d7d75-205">SaveChanges tenta definir a postagem FK como nulo, mas falha porque a CE não é anulável</span><span class="sxs-lookup"><span data-stu-id="d7d75-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="d7d75-206">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional</span><span class="sxs-lookup"><span data-stu-id="d7d75-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="d7d75-207">Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="d7d75-208">Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="d7d75-209">SaveChanges define FK (postagens) em filhos dependentes/como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="d7d75-210">Depois de salvar, dependentes/filhos (postagens) agora tem valores nulos de FK e sua referência para a entidade de segurança/pai excluído (blog) foi removida</span><span class="sxs-lookup"><span data-stu-id="d7d75-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="d7d75-211">DeleteBehavior.Restrict com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="d7d75-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="d7d75-212">Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="d7d75-213">Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="d7d75-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="d7d75-214">Como *restringir* informa ao EF para definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges gera sem salvar</span><span class="sxs-lookup"><span data-stu-id="d7d75-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="d7d75-215">Em cascata para entidades não controladas</span><span class="sxs-lookup"><span data-stu-id="d7d75-215">Cascading to untracked entities</span></span>

<span data-ttu-id="d7d75-216">Quando você chama *SaveChanges*, as regras de exclusão em cascata serão aplicadas a qualquer entidade que está sendo controlada pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="d7d75-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="d7d75-217">Esta é a situação em todos os exemplos mostrados acima, que é por SQL foi gerado para excluir a entidade de segurança/pai (blog) e todos os seus dependentes/filhos (postagens):</span><span class="sxs-lookup"><span data-stu-id="d7d75-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="d7d75-218">Se apenas a entidade de segurança é carregada – por exemplo, quando uma consulta for feita para um blog sem um `Include(b => b.Posts)` para incluir também as postagens – em seguida, SaveChanges gerará somente o SQL para excluir a entidade de segurança/pai:</span><span class="sxs-lookup"><span data-stu-id="d7d75-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="d7d75-219">Os dependentes/filhos (postagens) só serão excluídos se o banco de dados tem um comportamento de cascata correspondente configurado.</span><span class="sxs-lookup"><span data-stu-id="d7d75-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="d7d75-220">Se você usar o EF para criar o banco de dados, esse comportamento em cascata será instalado para você.</span><span class="sxs-lookup"><span data-stu-id="d7d75-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
