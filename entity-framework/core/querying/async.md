---
title: Consultas assíncronas – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417749"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="58651-102">Consultas assíncronas</span><span class="sxs-lookup"><span data-stu-id="58651-102">Asynchronous Queries</span></span>

<span data-ttu-id="58651-103">As consultas assíncronas evitam o bloqueio de um thread enquanto a consulta é executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="58651-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="58651-104">Consultas assíncronas são importantes para manter uma interface do usuário responsiva em aplicativos cliente espesso.</span><span class="sxs-lookup"><span data-stu-id="58651-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="58651-105">Eles também podem aumentar a taxa de transferência em aplicativos Web onde liberam o thread para atender a outras solicitações em aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="58651-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="58651-106">Para saber mais, veja [Programação assíncrona em C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="58651-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="58651-107">EF Core não dá suporte a várias operações paralelas sendo executadas na mesma instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="58651-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="58651-108">Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte.</span><span class="sxs-lookup"><span data-stu-id="58651-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="58651-109">Isso normalmente é feito usando a palavra-chave `await` em cada operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="58651-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="58651-110">Entity Framework Core fornece um conjunto de métodos de extensão assíncrona semelhante aos métodos LINQ, que executam uma consulta e retornam resultados.</span><span class="sxs-lookup"><span data-stu-id="58651-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="58651-111">Os exemplos incluem `ToListAsync()`, `ToArrayAsync()``SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="58651-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="58651-112">Não há versões assíncronas de alguns operadores LINQ, como `Where(...)` ou `OrderBy(...)` porque esses métodos só criam a árvore de expressões LINQ e não fazem com que a consulta seja executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="58651-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="58651-113">Os métodos de extensão assíncrona do EF Core são definidos no namespace `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="58651-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="58651-114">Esse namespace deve ser importado para que os métodos sejam disponibilizados.</span><span class="sxs-lookup"><span data-stu-id="58651-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
