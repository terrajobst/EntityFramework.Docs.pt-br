---
title: Salvamento básico – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 6f72458504a9dbe99038af7cfd23b6991258f6b8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197780"
---
# <a name="basic-save"></a><span data-ttu-id="abc1b-102">Salvamento básico</span><span class="sxs-lookup"><span data-stu-id="abc1b-102">Basic Save</span></span>

<span data-ttu-id="abc1b-103">Saiba como adicionar, modificar e remover dados usando as classes de entidade e contexto.</span><span class="sxs-lookup"><span data-stu-id="abc1b-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="abc1b-104">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="abc1b-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="abc1b-105">Adicionando dados</span><span class="sxs-lookup"><span data-stu-id="abc1b-105">Adding Data</span></span>

<span data-ttu-id="abc1b-106">Use o método *DbSet.Add* para adicionar novas instâncias de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="abc1b-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="abc1b-107">Os dados serão inseridos no banco de dados quando você chamar *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="abc1b-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="abc1b-108">Os métodos Adicionar, Anexar e Atualizar funcionam no gráfico completo de entidades aprovadas para eles, conforme descrito na seção [Dados Relacionados](related-data.md).</span><span class="sxs-lookup"><span data-stu-id="abc1b-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="abc1b-109">Como alternativa, a propriedade EntityEntry.State pode ser usada para definir o estado de uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="abc1b-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="abc1b-110">Por exemplo, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="abc1b-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="abc1b-111">Atualizando dados</span><span class="sxs-lookup"><span data-stu-id="abc1b-111">Updating Data</span></span>

<span data-ttu-id="abc1b-112">O EF detectará automaticamente as alterações feitas em uma entidade existente que é controlada pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="abc1b-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="abc1b-113">Isso inclui as entidades que você carrega/consulta do banco de dados e as entidades que foram anteriormente adicionadas e salvas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="abc1b-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="abc1b-114">Basta modificar os valores atribuídos às propriedades e chamar *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="abc1b-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="abc1b-115">Excluindo dados</span><span class="sxs-lookup"><span data-stu-id="abc1b-115">Deleting Data</span></span>

<span data-ttu-id="abc1b-116">Use o método *DbSet.Remove* para excluir instâncias das suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="abc1b-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="abc1b-117">Se a entidade já existir no banco de dados, ela será excluída durante *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="abc1b-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="abc1b-118">Se a entidade ainda não tiver sido salva no banco de dados (ou seja, ela for rastreada como adicionada), ela será removida do contexto e não será mais inserida quando *SaveChanges* for chamado.</span><span class="sxs-lookup"><span data-stu-id="abc1b-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="abc1b-119">Várias operações em um único SaveChanges</span><span class="sxs-lookup"><span data-stu-id="abc1b-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="abc1b-120">Você pode combinar várias operações Adicionar/Carregar/Remover em uma única chamada para *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="abc1b-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="abc1b-121">Para a maioria dos provedores de banco de dados, *SaveChanges* é transacional.</span><span class="sxs-lookup"><span data-stu-id="abc1b-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="abc1b-122">Isso significa que todas as operações serão bem-sucedidas ou falharão e as operações nunca ficarão parcialmente aplicadas.</span><span class="sxs-lookup"><span data-stu-id="abc1b-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
