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
# <a name="asynchronous-saving"></a>Salvar assíncrona

Salvar assíncrona evita o bloqueio de um thread enquanto as alterações são gravadas no banco de dados. Isso pode ser útil para evitar o congelamento de interface do usuário de um aplicativo cliente grossa. Operações assíncronas também pode aumentar a taxa de transferência em um aplicativo web, em que o thread pode ser liberado para atender a outras solicitações enquanto a operação de banco de dados é concluída. Para obter mais informações, consulte [programação assíncrona em c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Núcleo EF não oferece suporte a várias operações simultâneas que estão sendo executadas na mesma instância do contexto. Você sempre deve esperar para uma operação seja concluída antes de iniciar a próxima operação. Isso geralmente é feito usando o `await` palavra-chave em cada operação assíncrona.

Entity Framework Core fornece `DbContext.SaveChangesAsync()` como uma alternativa assíncrona para `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
