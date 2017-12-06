---
title: Tokens de simultaneidade - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="78293-102">Tokens de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="78293-102">Concurrency Tokens</span></span>

<span data-ttu-id="78293-103">Se uma propriedade é configurada como um token de simultaneidade EF irá verificar se nenhum outro usuário modificou esse valor no banco de dados ao salvar alterações no registro.</span><span class="sxs-lookup"><span data-stu-id="78293-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="78293-104">EF usa um padrão de simultaneidade otimista, que significa que ela será assumem que o valor tenha sido alterada e tentar salvar os dados, mas geram se encontra que o valor foi alterado.</span><span class="sxs-lookup"><span data-stu-id="78293-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="78293-105">Por exemplo, talvez você queira configurar `LastName` em `Person` um token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="78293-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="78293-106">Isso significa que, se um usuário tenta salvar algumas alterações em um `Person`, mas outro usuário alterou o `LastName` , em seguida, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="78293-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="78293-107">Isso pode ser desejável para que seu aplicativo pode solicitar ao usuário para garantir que este registro ainda representa a mesma pessoa real antes de salvar suas alterações.</span><span class="sxs-lookup"><span data-stu-id="78293-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="78293-108">Esta página documenta como configurar tokens de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="78293-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="78293-109">Consulte [tratamento simultaneidade](../saving/concurrency.md) para obter exemplos de como usar a simultaneidade otimista em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78293-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="78293-110">Como os tokens de simultaneidade funcionam no EF</span><span class="sxs-lookup"><span data-stu-id="78293-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="78293-111">Armazenamentos de dados podem impor tokens de simultaneidade, verificando se qualquer registro que está sendo atualizada ou excluída ainda tem o mesmo valor para o token de simultaneidade que foi atribuído quando o contexto originalmente carregou os dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="78293-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="78293-112">Por exemplo, os bancos de dados relacionais fazer isso, incluindo o token de simultaneidade no `WHERE` cláusula de qualquer `UPDATE` ou `DELETE` comandos e verificando o número de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="78293-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="78293-113">Se o token de simultaneidade ainda corresponde a uma linha será atualizada.</span><span class="sxs-lookup"><span data-stu-id="78293-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="78293-114">Se o valor no banco de dados foi alterada, nenhuma linha é atualizada.</span><span class="sxs-lookup"><span data-stu-id="78293-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="78293-115">Convenções</span><span class="sxs-lookup"><span data-stu-id="78293-115">Conventions</span></span>

<span data-ttu-id="78293-116">Por convenção, as propriedades nunca são configuradas como tokens de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="78293-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="78293-117">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="78293-117">Data Annotations</span></span>

<span data-ttu-id="78293-118">Você pode usar as anotações de dados para configurar uma propriedade como um token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="78293-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="78293-119">API fluente</span><span class="sxs-lookup"><span data-stu-id="78293-119">Fluent API</span></span>

<span data-ttu-id="78293-120">Você pode usar a API fluente para configurar uma propriedade como um token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="78293-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="78293-121">Versão de carimbo de hora/linha</span><span class="sxs-lookup"><span data-stu-id="78293-121">Timestamp/row version</span></span>

<span data-ttu-id="78293-122">Um carimbo de hora é uma propriedade em que um novo valor é gerado pelo banco de dados sempre que uma linha é inserida ou atualizada.</span><span class="sxs-lookup"><span data-stu-id="78293-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="78293-123">A propriedade também é tratada como um token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="78293-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="78293-124">Isso garante que você receberá uma exceção se ninguém tiver modificado uma linha que você está tentando atualizar desde consultada para os dados.</span><span class="sxs-lookup"><span data-stu-id="78293-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="78293-125">Como isso é obtido é até o provedor de banco de dados que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="78293-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="78293-126">Para o SQL Server, carimbo de hora é geralmente usado em uma *byte []* propriedade, que será de instalação como um *ROWVERSION* coluna no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="78293-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="78293-127">Convenções</span><span class="sxs-lookup"><span data-stu-id="78293-127">Conventions</span></span>

<span data-ttu-id="78293-128">Por convenção, as propriedades nunca são configuradas como os carimbos de hora.</span><span class="sxs-lookup"><span data-stu-id="78293-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="78293-129">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="78293-129">Data Annotations</span></span>

<span data-ttu-id="78293-130">Você pode usar as anotações de dados para configurar uma propriedade como um carimbo de hora.</span><span class="sxs-lookup"><span data-stu-id="78293-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="78293-131">API fluente</span><span class="sxs-lookup"><span data-stu-id="78293-131">Fluent API</span></span>

<span data-ttu-id="78293-132">Você pode usar a API fluente para configurar uma propriedade como um carimbo de hora.</span><span class="sxs-lookup"><span data-stu-id="78293-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
