---
title: Consultas assíncronas – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993559"
---
# <a name="asynchronous-queries"></a>Consultas assíncronas

As consultas assíncronas evitam o bloqueio de um thread enquanto a consulta é executada no banco de dados. Isso pode ser útil para evitar o congelamento da interface do usuário de um aplicativo de cliente espesso. As operações assíncronas também pode aumentar a taxa de transferência em um aplicativo Web, em que o thread pode ser liberado para atender a outras solicitações enquanto a operação do banco de dados é concluída. Para saber mais, veja [Programação assíncrona em C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> O EF Core não oferece suporte para várias operações simultâneas sendo executadas na mesma instância de contexto. Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte. Isso geralmente é feito usando a palavra-chave `await` em cada operação assíncrona.

O Entity Framework Core fornece um conjunto de métodos de extensão assíncrona que podem ser usados como uma alternativa para os métodos LINQ que fazem com que uma consulta seja executada e os resultados retornados. Os exemplo incluem `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` etc. Não há versões assíncronas dos operadores LINQ, como `Where(...)`, `OrderBy(...)` etc. porque esses métodos só são desenvolvidos na árvore de expressão do LINQ e não fazem com que a consulta seja executada no banco de dados.

> [!IMPORTANT]  
> Os métodos de extensão assíncrona do EF Core são definidos no namespace `Microsoft.EntityFrameworkCore`. Esse namespace deve ser importado para que os métodos sejam disponibilizados.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
