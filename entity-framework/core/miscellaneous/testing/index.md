---
title: "Testar componentes usando o Entity Framework – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="a82c5-102">Testes</span><span class="sxs-lookup"><span data-stu-id="a82c5-102">Testing</span></span>

<span data-ttu-id="a82c5-103">Você talvez queira testar componentes usando algo que se aproxime da conexão ao banco de dados real sem a sobrecarga de operações de E/S de banco de dados reais.</span><span class="sxs-lookup"><span data-stu-id="a82c5-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="a82c5-104">Há duas opções principais para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="a82c5-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="a82c5-105">O [Modo em memória do SQLite](sqlite.md) permite escrever testes eficientes para um provedor que se comporte como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="a82c5-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="a82c5-106">[O provedor InMemory](in-memory.md) é um provedor leve que tem dependências mínimas, mas nem sempre se comporta como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="a82c5-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
