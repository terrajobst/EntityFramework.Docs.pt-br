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
# <a name="asynchronous-queries"></a>Consultas assíncronas

As consultas assíncronas evitam o bloqueio de um thread enquanto a consulta é executada no banco de dados. As consultas de async são importantes para manter uma interface do usuário responsiva em aplicativos de cliente grosso. Eles também podem aumentar o throughput em aplicativos web onde eles liberam o thread para atender outras solicitações em aplicativos web. Para obter mais informações, consulte [Programação Assíncrona em C#](/dotnet/csharp/async).

> [!WARNING]  
> O EF Core não suporta várias operações paralelas sendo executadas na mesma instância de contexto. Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte. Isso é normalmente feito `await` usando a palavra-chave em cada operação de assincronia.

Entity Framework Core fornece um conjunto de métodos de extensão assíncrona semelhantes aos métodos LINQ, que executam resultados de consulta e retorno. Exemplos `ToListAsync()`incluem, `ToArrayAsync()` `SingleAsync()`, . Não há versões assinuosas `Where(...)` `OrderBy(...)` de alguns operadores linq, tais como ou porque esses métodos só constroem a árvore de expressão LINQ e não fazem com que a consulta seja executada no banco de dados.

> [!IMPORTANT]  
> Os métodos de extensão assíncrona do EF Core são definidos no namespace `Microsoft.EntityFrameworkCore`. Esse namespace deve ser importado para que os métodos sejam disponibilizados.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
