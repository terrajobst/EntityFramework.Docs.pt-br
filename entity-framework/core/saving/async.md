---
title: Salvamento assíncrono – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997276"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="b760e-102">Salvamento assíncrono</span><span class="sxs-lookup"><span data-stu-id="b760e-102">Asynchronous Saving</span></span>

<span data-ttu-id="b760e-103">O salvamento assíncrono evita o bloqueio de um thread enquanto as alterações são gravadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b760e-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="b760e-104">Isso pode ser útil para evitar o congelamento da interface do usuário de um aplicativo de cliente espesso.</span><span class="sxs-lookup"><span data-stu-id="b760e-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="b760e-105">As operações assíncronas também pode aumentar a taxa de transferência em um aplicativo Web, em que o thread pode ser liberado para atender a outras solicitações enquanto a operação do banco de dados é concluída.</span><span class="sxs-lookup"><span data-stu-id="b760e-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="b760e-106">Para saber mais, veja [Programação assíncrona em C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="b760e-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="b760e-107">O EF Core não oferece suporte para várias operações simultâneas sendo executadas na mesma instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="b760e-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="b760e-108">Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte.</span><span class="sxs-lookup"><span data-stu-id="b760e-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="b760e-109">Isso geralmente é feito usando a palavra-chave `await` em cada operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="b760e-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="b760e-110">O Entity Framework Core fornece `DbContext.SaveChangesAsync()` como uma alternativa assíncrona para `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="b760e-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
