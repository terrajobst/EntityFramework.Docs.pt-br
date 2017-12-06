---
title: "Salvar básico - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a><span data-ttu-id="5a1b5-102">Salvar básico</span><span class="sxs-lookup"><span data-stu-id="5a1b5-102">Basic Save</span></span>

<span data-ttu-id="5a1b5-103">Saiba como adicionar, modificar e remover dados usando as classes de entidade e de contexto.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="5a1b5-104">Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) no GitHub.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="5a1b5-105">Adicionando dados</span><span class="sxs-lookup"><span data-stu-id="5a1b5-105">Adding Data</span></span>

<span data-ttu-id="5a1b5-106">Use o *DbSet.Add* método para adicionar novas instâncias de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="5a1b5-107">Os dados serão inseridos no banco de dados quando você chama *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a><span data-ttu-id="5a1b5-108">Atualizando dados</span><span class="sxs-lookup"><span data-stu-id="5a1b5-108">Updating Data</span></span>

<span data-ttu-id="5a1b5-109">EF detectará automaticamente as alterações feitas a uma entidade existente que é controlada pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-109">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="5a1b5-110">Isso inclui entidades que você carregar/consulta do banco de dados e entidades que foram adicionadas anteriormente e salvos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-110">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="5a1b5-111">Basta modificar os valores atribuídos às propriedades e, em seguida, chamar *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-111">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="5a1b5-112">Excluindo dados</span><span class="sxs-lookup"><span data-stu-id="5a1b5-112">Deleting Data</span></span>

<span data-ttu-id="5a1b5-113">Use o *DbSet.Remove* método para excluir instâncias que classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-113">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="5a1b5-114">Se a entidade já existe no banco de dados, ele será excluído durante *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-114">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="5a1b5-115">Se a entidade ainda não foram salvas no banco de dados (ou seja, ele é controlado como adicionado) e em seguida, ele será removido do contexto e deixará de ser inserido quando *SaveChanges* é chamado.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-115">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="5a1b5-116">Várias operações em um único SaveChanges</span><span class="sxs-lookup"><span data-stu-id="5a1b5-116">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="5a1b5-117">Você pode combinar várias operações de atualização/Adicionar/remover em uma única chamada para *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-117">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="5a1b5-118">Para a maioria dos provedores de banco de dados, *SaveChanges* é transacional.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-118">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="5a1b5-119">Isso significa que todas as operações serão bem-sucedidas ou falham e as operações serão nunca esquerda parcialmente aplicadas.</span><span class="sxs-lookup"><span data-stu-id="5a1b5-119">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
