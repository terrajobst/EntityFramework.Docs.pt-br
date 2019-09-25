---
title: Testar componentes usando o EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197499"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="81883-102">Testar componentes usando o EF Core</span><span class="sxs-lookup"><span data-stu-id="81883-102">Testing components using EF Core</span></span>

<span data-ttu-id="81883-103">Você talvez queira testar componentes usando algo que se aproxime da conexão ao banco de dados real sem a sobrecarga de operações de E/S de banco de dados reais.</span><span class="sxs-lookup"><span data-stu-id="81883-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="81883-104">Há duas opções principais para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="81883-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="81883-105">O [Modo em memória do SQLite](sqlite.md) permite escrever testes eficientes para um provedor que se comporte como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="81883-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="81883-106">[O provedor InMemory](in-memory.md) é um provedor leve que tem dependências mínimas, mas nem sempre se comporta como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="81883-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
