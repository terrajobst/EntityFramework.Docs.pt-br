---
title: Excluir em cascata – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812671"
---
# <a name="cascade-delete"></a><span data-ttu-id="ae993-102">Excluir em cascata</span><span class="sxs-lookup"><span data-stu-id="ae993-102">Cascade Delete</span></span>

<span data-ttu-id="ae993-103">A Exclusão em cascata é geralmente usada na terminologia de banco de dados para descrever uma característica que permite a exclusão de uma linha para disparar automaticamente a exclusão de linhas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ae993-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="ae993-104">Um conceito relacionado também coberto por comportamentos de exclusão de EF Core é a exclusão automática de uma entidade filho quando sua relação com um pai foi desfeita, isso é conhecido como "excluir órfãos".</span><span class="sxs-lookup"><span data-stu-id="ae993-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="ae993-105">O EF Core implementa vários comportamentos de exclusão diferentes e permite a configuração dos comportamentos de exclusão de relações individuais.</span><span class="sxs-lookup"><span data-stu-id="ae993-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="ae993-106">O EF Core também implementa convenções que configuram automaticamente os comportamentos de exclusão padrão úteis para cada relação com base nas [exigências do relacionamento](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="ae993-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="ae993-107">Comportamentos de exclusão</span><span class="sxs-lookup"><span data-stu-id="ae993-107">Delete behaviors</span></span>
<span data-ttu-id="ae993-108">Os comportamentos de exclusão são definidos no tipo de enumerador *DeleteBehavior* e pode ser passado para a API fluente *OnDelete* para controlar se a exclusão de uma entidade de segurança/pai ou o corte da relação com entidades dependentes/filho deve ter um efeito colateral nas entidades dependentes/filho.</span><span class="sxs-lookup"><span data-stu-id="ae993-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="ae993-109">Há três ações que o EF pode executar quando uma entidade de segurança/pai é excluída ou a relação com o filho é desligada:</span><span class="sxs-lookup"><span data-stu-id="ae993-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="ae993-110">O filho/dependente pode ser excluído</span><span class="sxs-lookup"><span data-stu-id="ae993-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="ae993-111">Os valores de chave estrangeira do filho podem ser definidos como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="ae993-112">O filho permanece inalterado</span><span class="sxs-lookup"><span data-stu-id="ae993-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="ae993-113">O comportamento de exclusão configurado no modelo do EF Core só é aplicado quando a entidade de segurança é excluída usando o EF Core e as entidades dependentes são carregadas na memória (ou seja, para dependentes controlados).</span><span class="sxs-lookup"><span data-stu-id="ae993-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="ae993-114">Um comportamento em cascata correspondente precisa ser configurado no banco de dados para garantir que não esteja sendo controlado pelo contexto e tenha a ação necessária aplicada.</span><span class="sxs-lookup"><span data-stu-id="ae993-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="ae993-115">Se você usar o EF Core para criar o banco de dados, esse comportamento em cascata será configurado para você.</span><span class="sxs-lookup"><span data-stu-id="ae993-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="ae993-116">Para a segunda ação acima, definir um valor de chave estrangeira como nulo não será válido se a chave estrangeira não for anulável.</span><span class="sxs-lookup"><span data-stu-id="ae993-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="ae993-117">(Uma chave estrangeira não anulável é equivalente a uma relação obrigatória). Nesses casos, o Core EF controla se a propriedade de chave estrangeira foi marcada como nula até SaveChanges ser chamado, o momento em que uma exceção será gerada porque as alterações não podem ser mantidas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ae993-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="ae993-118">Isso é semelhante a obter uma violação de restrição do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ae993-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="ae993-119">Há quatro comportamentos de exclusão, conforme o listado nas tabelas a seguir.</span><span class="sxs-lookup"><span data-stu-id="ae993-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="ae993-120">Para relações opcionais (chave estrangeira anulável), _é_ possível salvar um valor de chave estrangeiro nulo, que resulta nos seguintes efeitos:</span><span class="sxs-lookup"><span data-stu-id="ae993-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="ae993-121">Nome do comportamento</span><span class="sxs-lookup"><span data-stu-id="ae993-121">Behavior Name</span></span>               | <span data-ttu-id="ae993-122">Efeito em dependente/filho na memória</span><span class="sxs-lookup"><span data-stu-id="ae993-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="ae993-123">Efeito em dependente/filho no banco de dados</span><span class="sxs-lookup"><span data-stu-id="ae993-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="ae993-124">**Cascata**</span><span class="sxs-lookup"><span data-stu-id="ae993-124">**Cascade**</span></span>                 | <span data-ttu-id="ae993-125">As entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="ae993-125">Entities are deleted</span></span>                   | <span data-ttu-id="ae993-126">As entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="ae993-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="ae993-127">**ClientSetNull** (padrão)</span><span class="sxs-lookup"><span data-stu-id="ae993-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="ae993-128">Propriedades de chave estrangeira são definidas como nulas</span><span class="sxs-lookup"><span data-stu-id="ae993-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="ae993-129">Nenhum</span><span class="sxs-lookup"><span data-stu-id="ae993-129">None</span></span>                                   |
| <span data-ttu-id="ae993-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="ae993-130">**SetNull**</span></span>                 | <span data-ttu-id="ae993-131">Propriedades de chave estrangeira são definidas como nulas</span><span class="sxs-lookup"><span data-stu-id="ae993-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="ae993-132">Propriedades de chave estrangeira são definidas como nulas</span><span class="sxs-lookup"><span data-stu-id="ae993-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="ae993-133">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="ae993-133">**Restrict**</span></span>                | <span data-ttu-id="ae993-134">Nenhum</span><span class="sxs-lookup"><span data-stu-id="ae993-134">None</span></span>                                   | <span data-ttu-id="ae993-135">Nenhum</span><span class="sxs-lookup"><span data-stu-id="ae993-135">None</span></span>                                   |

<span data-ttu-id="ae993-136">Para relações obrigatórias (chave estrangeira não anulável), _não_ pode salvar um valor de chave estrangeiro nulo, que resulta nos seguintes efeitos:</span><span class="sxs-lookup"><span data-stu-id="ae993-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="ae993-137">Nome do comportamento</span><span class="sxs-lookup"><span data-stu-id="ae993-137">Behavior Name</span></span>         | <span data-ttu-id="ae993-138">Efeito em dependente/filho na memória</span><span class="sxs-lookup"><span data-stu-id="ae993-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="ae993-139">Efeito em dependente/filho no banco de dados</span><span class="sxs-lookup"><span data-stu-id="ae993-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="ae993-140">**Cascata** (Padrão)</span><span class="sxs-lookup"><span data-stu-id="ae993-140">**Cascade** (Default)</span></span> | <span data-ttu-id="ae993-141">As entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="ae993-141">Entities are deleted</span></span>                | <span data-ttu-id="ae993-142">As entidades são excluídas</span><span class="sxs-lookup"><span data-stu-id="ae993-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="ae993-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="ae993-143">**ClientSetNull**</span></span>     | <span data-ttu-id="ae993-144">SaveChanges gera</span><span class="sxs-lookup"><span data-stu-id="ae993-144">SaveChanges throws</span></span>                  | <span data-ttu-id="ae993-145">Nenhum</span><span class="sxs-lookup"><span data-stu-id="ae993-145">None</span></span>                                  |
| <span data-ttu-id="ae993-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="ae993-146">**SetNull**</span></span>           | <span data-ttu-id="ae993-147">SaveChanges gera</span><span class="sxs-lookup"><span data-stu-id="ae993-147">SaveChanges throws</span></span>                  | <span data-ttu-id="ae993-148">SaveChanges gera</span><span class="sxs-lookup"><span data-stu-id="ae993-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="ae993-149">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="ae993-149">**Restrict**</span></span>          | <span data-ttu-id="ae993-150">Nenhum</span><span class="sxs-lookup"><span data-stu-id="ae993-150">None</span></span>                                | <span data-ttu-id="ae993-151">Nenhum</span><span class="sxs-lookup"><span data-stu-id="ae993-151">None</span></span>                                  |

<span data-ttu-id="ae993-152">Nas tabelas acima, *Nenhum* pode resultar em uma violação de restrição.</span><span class="sxs-lookup"><span data-stu-id="ae993-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="ae993-153">Por exemplo, se uma entidade de segurança/filho for excluída, mas nenhuma ação for tomada para alterar a chave estrangeira de um dependente/filho, então o banco de dados provavelmente gerará SaveChanges devido a uma violação de restrição de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="ae993-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="ae993-154">Em um alto nível:</span><span class="sxs-lookup"><span data-stu-id="ae993-154">At a high level:</span></span>
* <span data-ttu-id="ae993-155">Se você tiver entidades que não podem existir sem um pai, e você deseja que o EF cuide para excluir os filhos automaticamente, use *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="ae993-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="ae993-156">As entidades que não podem existir sem um pai geralmente usam as relações obrigatórias, para as quais *Cascade* é o padrão.</span><span class="sxs-lookup"><span data-stu-id="ae993-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="ae993-157">Se você tiver entidades que podem ou não ter um pai, e deseja que o EF cuide de anular a chave estrangeira para você, use *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="ae993-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="ae993-158">As entidades que podem existir sem um pai geralmente usam as relações opcionais, para as quais *ClientSetNull* é o padrão.</span><span class="sxs-lookup"><span data-stu-id="ae993-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="ae993-159">Se você quiser que o banco de dados também tente propagar os valores nulos para chaves estrangeiras filho até mesmo quando a entidade filho não está carregada, em seguida, use *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="ae993-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="ae993-160">No entanto, observe que o banco de dados deve oferecer suporte a isso e configurar o banco de dados assim pode resultar em outras restrições, que, na prática, geralmente tornam essa opção impraticável.</span><span class="sxs-lookup"><span data-stu-id="ae993-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="ae993-161">É por isso que *SetNull* não é o padrão.</span><span class="sxs-lookup"><span data-stu-id="ae993-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="ae993-162">Se você não quiser que o EF Core exclua uma entidade automaticamente ou anule automaticamente a chave estrangeira, use *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="ae993-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="ae993-163">Observe que isso requer que seu código mantenha entidades filho e seus valores de chave estrangeira em sincronia manualmente, caso contrário, as exceções de restrição serão geradas.</span><span class="sxs-lookup"><span data-stu-id="ae993-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="ae993-164">No EF Core, ao contrário de EF6, os efeitos em cascata não ocorrem imediatamente, apenas quando SaveChanges é chamado.</span><span class="sxs-lookup"><span data-stu-id="ae993-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="ae993-165">**Alterações no EF Core 2.0:** em versões anteriores, *Restrict* causaria as propriedades de chave estrangeira opcionais em entidades dependentes controladas serem definidas como nulas e o padrão era o comportamento de exclusão para relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="ae993-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="ae993-166">No EF Core 2.0, o *ClientSetNull* foi introduzido para representar esse comportamento e tornou-se o padrão para relações opcionais.</span><span class="sxs-lookup"><span data-stu-id="ae993-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="ae993-167">O comportamento de *Restrict* foi ajustado para nunca ter efeitos colaterais em entidades dependentes.</span><span class="sxs-lookup"><span data-stu-id="ae993-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="ae993-168">Exemplos de exclusão de entidade</span><span class="sxs-lookup"><span data-stu-id="ae993-168">Entity deletion examples</span></span>

<span data-ttu-id="ae993-169">O código a seguir faz parte de um [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que pode ser baixado e executado.</span><span class="sxs-lookup"><span data-stu-id="ae993-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="ae993-170">O exemplo mostra o que acontece para cada comportamento de exclusão para relações obrigatórias e opcionais quando uma entidade pai é excluída.</span><span class="sxs-lookup"><span data-stu-id="ae993-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="ae993-171">Vamos examinar cada variação para entender o que está acontecendo.</span><span class="sxs-lookup"><span data-stu-id="ae993-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="ae993-172">DeleteBehavior.Cascade com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="ae993-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="ae993-173">O blog está marcado como Excluído</span><span class="sxs-lookup"><span data-stu-id="ae993-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="ae993-174">As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ae993-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="ae993-175">SaveChanges envia exclusões para dependentes/filhos (postagens) e, em seguida, a entidade de segurança/pai (blog)</span><span class="sxs-lookup"><span data-stu-id="ae993-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="ae993-176">Depois de salvar, todas as entidades serão desanexadas porque agora foram excluídas do banco de dados</span><span class="sxs-lookup"><span data-stu-id="ae993-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="ae993-177">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação obrigatória</span><span class="sxs-lookup"><span data-stu-id="ae993-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="ae993-178">O blog está marcado como Excluído</span><span class="sxs-lookup"><span data-stu-id="ae993-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="ae993-179">As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ae993-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="ae993-180">SaveChanges tenta definir a postagem FK como nula, mas falha porque a FK não é anulável</span><span class="sxs-lookup"><span data-stu-id="ae993-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="ae993-181">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional</span><span class="sxs-lookup"><span data-stu-id="ae993-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="ae993-182">O blog está marcado como Excluído</span><span class="sxs-lookup"><span data-stu-id="ae993-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="ae993-183">As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ae993-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="ae993-184">SaveChanges tenta definir FK de filhos/dependentes (postagens) como nulo antes de excluir a entidade de segurança/pai (blog)</span><span class="sxs-lookup"><span data-stu-id="ae993-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="ae993-185">Depois de salvar, a entidade de segurança/pai (blog) será excluída, mas os seus dependentes/filhos (postagens) ainda são controlados</span><span class="sxs-lookup"><span data-stu-id="ae993-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="ae993-186">Os dependentes/filhos controlados (postagens) agora têm valores FK nulos e sua referência para a entidade de segurança/pai excluída (blog) foi removida</span><span class="sxs-lookup"><span data-stu-id="ae993-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="ae993-187">DeleteBehavior.Restrict com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="ae993-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="ae993-188">O blog está marcado como Excluído</span><span class="sxs-lookup"><span data-stu-id="ae993-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="ae993-189">As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ae993-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="ae993-190">Como *Restrict* informa ao EF para não definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges é gerada sem salvar</span><span class="sxs-lookup"><span data-stu-id="ae993-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="ae993-191">Exemplos de exclusão de órfãos</span><span class="sxs-lookup"><span data-stu-id="ae993-191">Delete orphans examples</span></span>

<span data-ttu-id="ae993-192">O código a seguir faz parte de um [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que pode ser baixado e executado.</span><span class="sxs-lookup"><span data-stu-id="ae993-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="ae993-193">O exemplo mostra o que acontece para cada comportamento de exclusão de relações obrigatórias e opcionais quando a relação entre um pai/entidade de segurança e seus filhos/dependentes for desfeita.</span><span class="sxs-lookup"><span data-stu-id="ae993-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="ae993-194">Neste exemplo, a relação é desfeita removendo os dependentes/filhos (postagens) da propriedade de navegação da coleção na entidade de segurança/pai (blog).</span><span class="sxs-lookup"><span data-stu-id="ae993-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="ae993-195">No entanto, o comportamento é o mesmo se a referência de dependente/filho a entidade de segurança/pai em vez disso for anulada.</span><span class="sxs-lookup"><span data-stu-id="ae993-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="ae993-196">Vamos examinar cada variação para entender o que está acontecendo.</span><span class="sxs-lookup"><span data-stu-id="ae993-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="ae993-197">DeleteBehavior.Cascade com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="ae993-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="ae993-198">As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="ae993-199">Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="ae993-200">SaveChanges envia exclusões para dependentes/filhos (postagens)</span><span class="sxs-lookup"><span data-stu-id="ae993-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="ae993-201">Depois de salvar, os dependentes/filhos (postagens) serão desanexados porque agora foram excluídos do banco de dados</span><span class="sxs-lookup"><span data-stu-id="ae993-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="ae993-202">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação obrigatória</span><span class="sxs-lookup"><span data-stu-id="ae993-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="ae993-203">As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="ae993-204">Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="ae993-205">SaveChanges tenta definir a postagem FK como nula, mas falha porque a FK não é anulável</span><span class="sxs-lookup"><span data-stu-id="ae993-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="ae993-206">DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional</span><span class="sxs-lookup"><span data-stu-id="ae993-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="ae993-207">As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="ae993-208">Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="ae993-209">SaveChanges define FK de filhos/dependentes (postagens) como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="ae993-210">Depois de salvar, os dependentes/filhos (postagens) agora têm valores FK nulos e sua referência para a entidade de segurança/pai excluída (blog) foi removida</span><span class="sxs-lookup"><span data-stu-id="ae993-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="ae993-211">DeleteBehavior.Restrict com relação obrigatória ou opcional</span><span class="sxs-lookup"><span data-stu-id="ae993-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="ae993-212">As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="ae993-213">Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo</span><span class="sxs-lookup"><span data-stu-id="ae993-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="ae993-214">Como *Restrict* informa ao EF para não definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges é gerada sem salvar</span><span class="sxs-lookup"><span data-stu-id="ae993-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="ae993-215">Em cascata para entidades sem controle</span><span class="sxs-lookup"><span data-stu-id="ae993-215">Cascading to untracked entities</span></span>

<span data-ttu-id="ae993-216">Quando você chama *SaveChanges*, as regras de exclusão em cascata serão aplicadas a qualquer entidade que está sendo controlada pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="ae993-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="ae993-217">Esta é a situação em todos os exemplos mostrados acima, que é por que o SQL foi gerado para excluir a entidade de segurança/pai (blog) e todos os seus dependentes/filhos (postagens):</span><span class="sxs-lookup"><span data-stu-id="ae993-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="ae993-218">Se apenas a entidade de segurança for carregada, por exemplo, quando uma consulta for feita para um blog sem um `Include(b => b.Posts)` para incluir também as postagens, SaveChanges apenas gerará o SQL para excluir a entidade de segurança/pai:</span><span class="sxs-lookup"><span data-stu-id="ae993-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="ae993-219">Os dependentes/filhos (postagens) só serão excluídos se o banco de dados tiver um comportamento em cascata correspondente configurado.</span><span class="sxs-lookup"><span data-stu-id="ae993-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="ae993-220">Se você usar o EF para criar o banco de dados, esse comportamento em cascata será configurado para você.</span><span class="sxs-lookup"><span data-stu-id="ae993-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
