---
title: Consultas assíncronas – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921689"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="ddd33-102">Consultas assíncronas</span><span class="sxs-lookup"><span data-stu-id="ddd33-102">Asynchronous Queries</span></span>

<span data-ttu-id="ddd33-103">As consultas assíncronas evitam o bloqueio de um thread enquanto a consulta é executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ddd33-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="ddd33-104">Isso pode ser útil para evitar o congelamento da interface do usuário de um aplicativo de cliente espesso.</span><span class="sxs-lookup"><span data-stu-id="ddd33-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="ddd33-105">As operações assíncronas também podem aumentar a taxa de transferência em um aplicativo Web, em que a thread pode ser liberada para atender a outras solicitações enquanto a operação do banco de dados é concluída.</span><span class="sxs-lookup"><span data-stu-id="ddd33-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="ddd33-106">Para saber mais, veja [Programação assíncrona em C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="ddd33-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="ddd33-107">O EF Core não oferece suporte para várias operações simultâneas sendo executadas na mesma instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="ddd33-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="ddd33-108">Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte.</span><span class="sxs-lookup"><span data-stu-id="ddd33-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="ddd33-109">Isso geralmente é feito usando a palavra-chave `await` em cada operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ddd33-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="ddd33-110">O Entity Framework Core fornece um conjunto de métodos de extensão assíncrona que podem ser usados como uma alternativa para os métodos LINQ que fazem com que uma consulta seja executada e os resultados retornados.</span><span class="sxs-lookup"><span data-stu-id="ddd33-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="ddd33-111">Os exemplo incluem `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` etc. Não há versões assíncronas dos operadores LINQ, como `Where(...)`, `OrderBy(...)` etc. porque esses métodos só são desenvolvidos na árvore de expressão do LINQ e não fazem com que a consulta seja executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ddd33-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ddd33-112">Os métodos de extensão assíncrona do EF Core são definidos no namespace `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="ddd33-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="ddd33-113">Esse namespace deve ser importado para que os métodos sejam disponibilizados.</span><span class="sxs-lookup"><span data-stu-id="ddd33-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
