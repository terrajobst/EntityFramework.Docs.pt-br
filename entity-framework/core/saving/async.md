---
title: Salvamento assíncrono – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197826"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="65e4f-102">Salvamento assíncrono</span><span class="sxs-lookup"><span data-stu-id="65e4f-102">Asynchronous Saving</span></span>

<span data-ttu-id="65e4f-103">O salvamento assíncrono evita o bloqueio de uma thread enquanto as alterações são gravadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="65e4f-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="65e4f-104">Isso pode ser útil para evitar o congelamento da interface do usuário de um aplicativo de cliente espesso.</span><span class="sxs-lookup"><span data-stu-id="65e4f-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="65e4f-105">As operações assíncronas também podem aumentar a taxa de transferência em um aplicativo Web, em que a thread pode ser liberada para atender a outras solicitações enquanto a operação do banco de dados é concluída.</span><span class="sxs-lookup"><span data-stu-id="65e4f-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="65e4f-106">Para saber mais, veja [Programação assíncrona em C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="65e4f-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="65e4f-107">O EF Core não oferece suporte para várias operações simultâneas sendo executadas na mesma instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="65e4f-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="65e4f-108">Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte.</span><span class="sxs-lookup"><span data-stu-id="65e4f-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="65e4f-109">Isso geralmente é feito usando a palavra-chave `await` em cada operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="65e4f-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="65e4f-110">O Entity Framework Core fornece `DbContext.SaveChangesAsync()` como uma alternativa assíncrona para `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="65e4f-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
