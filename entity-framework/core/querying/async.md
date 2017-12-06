---
title: "Consultas assíncronas - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Consultas assíncronas

Consultas assíncronas evitar o bloqueio de um thread enquanto a consulta é executada no banco de dados. Isso pode ser útil para evitar o congelamento de interface do usuário de um aplicativo cliente grossa. Operações assíncronas também pode aumentar a taxa de transferência em um aplicativo web, em que o thread pode ser liberado para atender a outras solicitações enquanto a operação de banco de dados é concluída. Para obter mais informações, consulte [programação assíncrona em c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Núcleo EF não oferece suporte a várias operações simultâneas que estão sendo executadas na mesma instância do contexto. Você sempre deve esperar para uma operação seja concluída antes de iniciar a próxima operação. Isso geralmente é feito usando o `await` palavra-chave em cada operação assíncrona.

Entity Framework Core fornece um conjunto de métodos de extensão assíncrona que pode ser usado como uma alternativa para os métodos LINQ que causam uma consulta a ser executada e os resultados retornados. Os exemplos incluem `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. Não há versões assíncronas dos operadores LINQ como `Where(...)`, `OrderBy(...)`, etc., pois esses métodos apenas criar a árvore de expressão LINQ e fazem com que a consulta a ser executada no banco de dados.

> [!IMPORTANT]  
> Os métodos de extensão do EF Core assíncronas são definidos no `Microsoft.EntityFrameworkCore` namespace. Esse namespace deve ser importado para os métodos disponíveis.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
