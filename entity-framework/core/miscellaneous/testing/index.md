---
title: Testar componentes usando o Entity Framework – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997831"
---
# <a name="testing"></a><span data-ttu-id="e86e2-102">Testes</span><span class="sxs-lookup"><span data-stu-id="e86e2-102">Testing</span></span>

<span data-ttu-id="e86e2-103">Você talvez queira testar componentes usando algo que se aproxime da conexão ao banco de dados real sem a sobrecarga de operações de E/S de banco de dados reais.</span><span class="sxs-lookup"><span data-stu-id="e86e2-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="e86e2-104">Há duas opções principais para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="e86e2-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="e86e2-105">O [Modo em memória do SQLite](sqlite.md) permite escrever testes eficientes para um provedor que se comporte como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="e86e2-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="e86e2-106">[O provedor InMemory](in-memory.md) é um provedor leve que tem dependências mínimas, mas nem sempre se comporta como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="e86e2-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
