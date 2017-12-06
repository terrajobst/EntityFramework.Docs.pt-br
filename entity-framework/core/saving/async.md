---
title: "Salvar assíncrona - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="8c138-102">Salvar assíncrona</span><span class="sxs-lookup"><span data-stu-id="8c138-102">Asynchronous Saving</span></span>

<span data-ttu-id="8c138-103">Salvar assíncrona evita o bloqueio de um thread enquanto as alterações são gravadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8c138-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="8c138-104">Isso pode ser útil para evitar o congelamento de interface do usuário de um aplicativo cliente grossa.</span><span class="sxs-lookup"><span data-stu-id="8c138-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="8c138-105">Operações assíncronas também pode aumentar a taxa de transferência em um aplicativo web, em que o thread pode ser liberado para atender a outras solicitações enquanto a operação de banco de dados é concluída.</span><span class="sxs-lookup"><span data-stu-id="8c138-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="8c138-106">Para obter mais informações, consulte [programação assíncrona em c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="8c138-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="8c138-107">Núcleo EF não oferece suporte a várias operações simultâneas que estão sendo executadas na mesma instância do contexto.</span><span class="sxs-lookup"><span data-stu-id="8c138-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="8c138-108">Você sempre deve esperar para uma operação seja concluída antes de iniciar a próxima operação.</span><span class="sxs-lookup"><span data-stu-id="8c138-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="8c138-109">Isso geralmente é feito usando o `await` palavra-chave em cada operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="8c138-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="8c138-110">Entity Framework Core fornece `DbContext.SaveChangesAsync()` como uma alternativa assíncrona para `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="8c138-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
