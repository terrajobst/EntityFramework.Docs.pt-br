---
title: Consultas assíncronas – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417749"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="517e0-102">Consultas assíncronas</span><span class="sxs-lookup"><span data-stu-id="517e0-102">Asynchronous Queries</span></span>

<span data-ttu-id="517e0-103">As consultas assíncronas evitam o bloqueio de um thread enquanto a consulta é executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="517e0-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="517e0-104">As consultas de async são importantes para manter uma interface do usuário responsiva em aplicativos de cliente grosso.</span><span class="sxs-lookup"><span data-stu-id="517e0-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="517e0-105">Eles também podem aumentar o throughput em aplicativos web onde eles liberam o thread para atender outras solicitações em aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="517e0-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="517e0-106">Para obter mais informações, consulte [Programação Assíncrona em C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="517e0-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="517e0-107">O EF Core não suporta várias operações paralelas sendo executadas na mesma instância de contexto.</span><span class="sxs-lookup"><span data-stu-id="517e0-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="517e0-108">Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte.</span><span class="sxs-lookup"><span data-stu-id="517e0-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="517e0-109">Isso é normalmente feito `await` usando a palavra-chave em cada operação de assincronia.</span><span class="sxs-lookup"><span data-stu-id="517e0-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="517e0-110">Entity Framework Core fornece um conjunto de métodos de extensão assíncrona semelhantes aos métodos LINQ, que executam resultados de consulta e retorno.</span><span class="sxs-lookup"><span data-stu-id="517e0-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="517e0-111">Exemplos `ToListAsync()`incluem, `ToArrayAsync()` `SingleAsync()`, .</span><span class="sxs-lookup"><span data-stu-id="517e0-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="517e0-112">Não há versões assinuosas `Where(...)` `OrderBy(...)` de alguns operadores linq, tais como ou porque esses métodos só constroem a árvore de expressão LINQ e não fazem com que a consulta seja executada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="517e0-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="517e0-113">Os métodos de extensão assíncrona do EF Core são definidos no namespace `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="517e0-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="517e0-114">Esse namespace deve ser importado para que os métodos sejam disponibilizados.</span><span class="sxs-lookup"><span data-stu-id="517e0-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
