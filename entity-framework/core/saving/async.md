---
title: Salvamento assíncrono – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417638"
---
# <a name="asynchronous-saving"></a>Salvamento assíncrono

O salvamento assíncrono evita o bloqueio de uma thread enquanto as alterações são gravadas no banco de dados. Isso pode ser útil para evitar o congelamento da interface do usuário de um aplicativo de cliente espesso. As operações assíncronas também podem aumentar a taxa de transferência em um aplicativo Web, em que a thread pode ser liberada para atender a outras solicitações enquanto a operação do banco de dados é concluída. Para obter mais informações, consulte [Programação Assíncrona em C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> O EF Core não oferece suporte para várias operações simultâneas sendo executadas na mesma instância de contexto. Aguarde sempre a conclusão de uma operação antes de iniciar a operação seguinte. Isso geralmente é feito usando a palavra-chave `await` em cada operação assíncrona.

O Entity Framework Core fornece `DbContext.SaveChangesAsync()` como uma alternativa assíncrona para `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
