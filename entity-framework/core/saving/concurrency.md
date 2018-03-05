---
title: Manipulando conflitos de simultaneidade - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="bd789-102">Manipulando conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="bd789-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="bd789-103">Esta página documenta como simultaneidade funciona no núcleo do EF e como lidar com conflitos de simultaneidade em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bd789-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="bd789-104">Consulte [Tokens de simultaneidade](xref:core/modeling/concurrency) para obter detalhes sobre como configurar os tokens de simultaneidade em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="bd789-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="bd789-105">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="bd789-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="bd789-106">_Simultaneidade de banco de dados_ refere-se a situações em que vários processos ou usuários acessarem ou alterem os mesmos dados em um banco de dados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="bd789-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="bd789-107">_Controle de simultaneidade_ refere-se aos mecanismos específicos usados para garantir a consistência de dados na presença de alterações simultâneas.</span><span class="sxs-lookup"><span data-stu-id="bd789-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="bd789-108">Implementa Core EF _controle de simultaneidade otimista_, que significa que ela permitirá que vários processos ou usuários alterarem independentemente, sem a sobrecarga de sincronização ou de bloqueio.</span><span class="sxs-lookup"><span data-stu-id="bd789-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="bd789-109">Em uma situação ideal, essas alterações não irá interferir com o outro e, portanto, poderá ser bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="bd789-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="bd789-110">Na pior das hipóteses, dois ou mais processos tentará fazer alterações em conflito e apenas um deles deve ter êxito.</span><span class="sxs-lookup"><span data-stu-id="bd789-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="bd789-111">Como funciona o controle de simultaneidade no núcleo do EF</span><span class="sxs-lookup"><span data-stu-id="bd789-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="bd789-112">As propriedades configuradas como tokens de simultaneidade são usados para implementar o controle de simultaneidade otimista: sempre que uma operação de atualização ou exclusão é executada durante a `SaveChanges`, o valor do token de simultaneidade no banco de dados é comparado com o original valor lido por núcleo EF.</span><span class="sxs-lookup"><span data-stu-id="bd789-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="bd789-113">Se os valores coincidirem, a operação pode ser concluída.</span><span class="sxs-lookup"><span data-stu-id="bd789-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="bd789-114">Se os valores não coincidirem, o núcleo EF presume que outro usuário executou uma operação conflitante e anula a transação atual.</span><span class="sxs-lookup"><span data-stu-id="bd789-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="bd789-115">A situação em que outro usuário executou uma operação que está em conflito com a operação atual é conhecida como _conflito de simultaneidade_.</span><span class="sxs-lookup"><span data-stu-id="bd789-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="bd789-116">Provedores de banco de dados serão responsáveis por implementar a comparação de valores de token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="bd789-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="bd789-117">Em bancos de dados relacionais EF Core inclui uma verificação para o valor do token de simultaneidade no `WHERE` cláusula de qualquer `UPDATE` ou `DELETE` instruções.</span><span class="sxs-lookup"><span data-stu-id="bd789-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="bd789-118">Depois de executar as instruções, Core EF lê o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="bd789-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="bd789-119">Se nenhuma linha for afetada, será detectado um conflito de simultaneidade e EF Core lança `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="bd789-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="bd789-120">Por exemplo, é aconselhável configurar `LastName` em `Person` um token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="bd789-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="bd789-121">Qualquer operação de atualização na pessoa incluirá a verificação de simultaneidade no `WHERE` cláusula:</span><span class="sxs-lookup"><span data-stu-id="bd789-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="bd789-122">Resolvendo conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="bd789-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="bd789-123">Continuando com o exemplo anterior, se um usuário tenta salvar algumas alterações em um `Person`, mas outro usuário já foi alterado o `LastName` a uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="bd789-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="bd789-124">Neste ponto, o aplicativo simplesmente pode informar ao usuário que a atualização não teve êxito devido a alterações conflitantes e Avançar.</span><span class="sxs-lookup"><span data-stu-id="bd789-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="bd789-125">Mas, talvez seja conveniente para solicitar ao usuário para garantir que este registro ainda representa a mesma pessoa real e tente a operação novamente.</span><span class="sxs-lookup"><span data-stu-id="bd789-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="bd789-126">Esse processo é um exemplo de _resolver um conflito de simultaneidade_.</span><span class="sxs-lookup"><span data-stu-id="bd789-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="bd789-127">Resolver um conflito de simultaneidade mesclando as alterações pendentes do atual `DbContext` com os valores no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd789-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="bd789-128">Quais valores são mesclados irão variar com base no aplicativo e podem ser direcionado pela entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="bd789-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="bd789-129">**Há três conjuntos de valores disponíveis para ajudar a resolver um conflito de simultaneidade:**</span><span class="sxs-lookup"><span data-stu-id="bd789-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="bd789-130">**Valores atuais** são os valores que o aplicativo estava tentando gravar no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd789-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="bd789-131">**Valores originais** são os valores que foram originalmente recuperados do banco de dados, antes de todas as edições feitas.</span><span class="sxs-lookup"><span data-stu-id="bd789-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="bd789-132">**Valores de banco de dados** são os valores atualmente armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd789-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="bd789-133">A abordagem geral para lidar com um conflito de simultaneidade é:</span><span class="sxs-lookup"><span data-stu-id="bd789-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="bd789-134">Catch `DbUpdateConcurrencyException` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bd789-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="bd789-135">Use `DbUpdateConcurrencyException.Entries` para preparar um novo conjunto de alterações para as entidades afetadas.</span><span class="sxs-lookup"><span data-stu-id="bd789-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="bd789-136">Atualize os valores originais do token de simultaneidade para refletir os valores atuais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bd789-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="bd789-137">Repita o processo até que nenhum conflito ocorre.</span><span class="sxs-lookup"><span data-stu-id="bd789-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="bd789-138">No exemplo a seguir, `Person.FirstName` e `Person.LastName` são configurados como tokens de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="bd789-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="bd789-139">Há um `// TODO:` comentário no local onde você incluir lógica específica do aplicativo para escolher o valor a ser salvo.</span><span class="sxs-lookup"><span data-stu-id="bd789-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
