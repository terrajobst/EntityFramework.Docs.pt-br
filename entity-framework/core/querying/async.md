---
title: Consultas assíncronas – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181817"
---
# <a name="asynchronous-queries"></a>Consultas assíncronas

As consultas assíncronas evitam o bloqueio de um thread enquanto a consulta é executada no banco de dados. Consultas assíncronas são importantes para manter uma interface do usuário responsiva em aplicativos cliente espesso. Eles também podem aumentar a taxa de transferência em aplicativos Web onde liberam o thread para atender a outras solicitações em aplicativos Web. Para saber mais, veja [Programação assíncrona em C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core não dá suporte a várias operações paralelas sendo executadas na mesma instância de contexto. Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte. Isso normalmente é feito usando a palavra-chave `await` em cada operação assíncrona.

Entity Framework Core fornece um conjunto de métodos de extensão assíncrona semelhante aos métodos LINQ, que executam uma consulta e retornam resultados. Os exemplos incluem `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`. Não há versões assíncronas de alguns operadores LINQ, como `Where(...)` ou `OrderBy(...)`, pois esses métodos só criam a árvore de expressões LINQ e não fazem com que a consulta seja executada no banco de dados.

> [!IMPORTANT]  
> Os métodos de extensão assíncrona do EF Core são definidos no namespace `Microsoft.EntityFrameworkCore`. Esse namespace deve ser importado para que os métodos sejam disponibilizados.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
