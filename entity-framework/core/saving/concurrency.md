---
title: Como tratar conflitos de simultaneidade – EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417586"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="a75bd-102">Como tratar conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="a75bd-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="a75bd-103">Esta página documenta como a simultaneidade funciona no EF Core e como lidar com conflitos de simultaneidade no seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a75bd-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="a75bd-104">Confira [Tokens de Simultaneidade](xref:core/modeling/concurrency) para obter detalhes sobre como configurar os tokens de simultaneidade no seu modelo.</span><span class="sxs-lookup"><span data-stu-id="a75bd-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="a75bd-105">Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="a75bd-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="a75bd-106">_Simultaneidade do banco de dados_ se refere a situações nas quais vários processos ou usuários acessam ou alteram os mesmos dados em um banco de dados ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="a75bd-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="a75bd-107">_Controle de simultaneidade_ se refere a mecanismos específicos usados para garantir a consistência dos dados na presença de alterações simultâneas.</span><span class="sxs-lookup"><span data-stu-id="a75bd-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="a75bd-108">O EF Core implementa o _controle de simultaneidade otimista_, o que significa que ele permitirá que vários processos ou usuários façam alterações de forma independente, sem a sobrecarga da sincronização ou bloqueio.</span><span class="sxs-lookup"><span data-stu-id="a75bd-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="a75bd-109">Em uma situação ideal, essas alterações não irão interferir umas nas outras e elas poderão ser bem-sucedidas.</span><span class="sxs-lookup"><span data-stu-id="a75bd-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="a75bd-110">Na pior das hipóteses, dois ou mais processos tentarão fazer alterações conflitantes e apenas uma delas terá êxito.</span><span class="sxs-lookup"><span data-stu-id="a75bd-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="a75bd-111">Como funciona o controle de simultaneidade no EF Core</span><span class="sxs-lookup"><span data-stu-id="a75bd-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="a75bd-112">As propriedades configuradas como tokens de simultaneidade são usadas para implementar um controle de simultaneidade otimista: sempre que uma operação de atualização ou exclusão é realizada durante `SaveChanges`, o valor do token de simultaneidade no banco de dados é comparado ao valor original lido pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="a75bd-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="a75bd-113">Se os valores coincidirem, a operação pode ser concluída.</span><span class="sxs-lookup"><span data-stu-id="a75bd-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="a75bd-114">Se os valores não coincidirem, o EF Core presume que outro usuário executou uma operação conflitante e anula a transação atual.</span><span class="sxs-lookup"><span data-stu-id="a75bd-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="a75bd-115">A situação quando outro usuário executou uma operação que conflita com a operação atual é conhecida como _conflito de simultaneidade_.</span><span class="sxs-lookup"><span data-stu-id="a75bd-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="a75bd-116">Os provedores do banco de dados são responsáveis por implementar a comparação dos valores do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="a75bd-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="a75bd-117">Em bancos de dados relacionais, o EF Core inclui uma verificação do valor do token de simultaneidade na cláusula `WHERE` de qualquer instrução `UPDATE` ou `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="a75bd-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="a75bd-118">Depois de executar as instruções, o EF Core lê o número de linhas que foram afetadas.</span><span class="sxs-lookup"><span data-stu-id="a75bd-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="a75bd-119">Se nenhuma linha for afetada, um conflito de simultaneidade será detectado e o EF Core gera `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="a75bd-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="a75bd-120">Por exemplo, é aconselhável configurar `LastName` em `Person` como um token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="a75bd-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="a75bd-121">Em seguida, qualquer operação de atualização em Pessoa incluirá a verificação de simultaneidade na cláusula `WHERE`:</span><span class="sxs-lookup"><span data-stu-id="a75bd-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="a75bd-122">Como resolver conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="a75bd-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="a75bd-123">Continuando com o exemplo anterior, se um usuário tentar salvar algumas alterações em um `Person`, mas outro usuário já tiver alterado o `LastName`, uma exceção será gerada.</span><span class="sxs-lookup"><span data-stu-id="a75bd-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="a75bd-124">Neste ponto, o aplicativo pode simplesmente informar ao usuário que a atualização não teve êxito devido a alterações conflitantes e avançar.</span><span class="sxs-lookup"><span data-stu-id="a75bd-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="a75bd-125">Mas, talvez seja desejável consultar o usuário para garantir que este registro represente a mesma pessoa e repetir a operação.</span><span class="sxs-lookup"><span data-stu-id="a75bd-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="a75bd-126">Este processo é um exemplo de como _resolver um conflito de simultaneidade_.</span><span class="sxs-lookup"><span data-stu-id="a75bd-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="a75bd-127">Resolver um conflito de simultaneidade envolve mesclar as alterações pendentes a partir do `DbContext` atual com os valores no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a75bd-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="a75bd-128">Os valores que serão mesclados irão variar com base no aplicativo e poderão ser direcionados pela entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="a75bd-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="a75bd-129">**Há três conjuntos de valores disponíveis para ajudar a resolver um conflito de simultaneidade:**</span><span class="sxs-lookup"><span data-stu-id="a75bd-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="a75bd-130">**Valores atuais** são os valores que o aplicativo estava tentando gravar no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a75bd-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="a75bd-131">**Valores originais** são os valores que foram originalmente recuperados do banco de dados, antes de todas as edições feitas.</span><span class="sxs-lookup"><span data-stu-id="a75bd-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="a75bd-132">**Valores do banco de dados** são os valores armazenados atualmente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a75bd-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="a75bd-133">A abordagem geral para lidar com um conflito de simultaneidade é:</span><span class="sxs-lookup"><span data-stu-id="a75bd-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="a75bd-134">Capturar `DbUpdateConcurrencyException` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a75bd-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="a75bd-135">Use `DbUpdateConcurrencyException.Entries` para preparar um novo conjunto de alterações para as entidades afetadas.</span><span class="sxs-lookup"><span data-stu-id="a75bd-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="a75bd-136">Atualize os valores originais do token de simultaneidade para refletir os valores atuais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a75bd-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="a75bd-137">Tente novamente o processo até o conflito ocorrer.</span><span class="sxs-lookup"><span data-stu-id="a75bd-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="a75bd-138">No exemplo a `Person.FirstName` `Person.LastName` seguir, e são configurados como tokens de concorrência.</span><span class="sxs-lookup"><span data-stu-id="a75bd-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="a75bd-139">Há um comentário `// TODO:` no local onde você inclui a lógica específica para o aplicativo para escolher o valor a ser salvo.</span><span class="sxs-lookup"><span data-stu-id="a75bd-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
