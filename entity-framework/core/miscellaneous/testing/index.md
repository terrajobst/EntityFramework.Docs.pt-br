---
title: Testar componentes usando o EF Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412791"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="06f4b-102">Testar componentes usando o EF Core</span><span class="sxs-lookup"><span data-stu-id="06f4b-102">Testing components using EF Core</span></span>

<span data-ttu-id="06f4b-103">Você talvez queira testar componentes usando algo que se aproxime da conexão ao banco de dados real sem a sobrecarga de operações de E/S de banco de dados reais.</span><span class="sxs-lookup"><span data-stu-id="06f4b-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="06f4b-104">Há duas opções principais para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="06f4b-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="06f4b-105">O [Modo em memória do SQLite](sqlite.md) permite escrever testes eficientes para um provedor que se comporte como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="06f4b-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="06f4b-106">[O provedor InMemory](in-memory.md) é um provedor leve que tem dependências mínimas, mas nem sempre se comporta como um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="06f4b-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
