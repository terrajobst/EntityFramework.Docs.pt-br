---
title: Salvar Dados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413091"
---
# <a name="saving-data"></a><span data-ttu-id="5e34a-102">Salvar Dados</span><span class="sxs-lookup"><span data-stu-id="5e34a-102">Saving Data</span></span>

<span data-ttu-id="5e34a-103">Cada instância de contexto tem um `ChangeTracker` que é responsável por manter o controle de alterações que precisam ser gravadas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5e34a-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="5e34a-104">Conforme você faz alterações a instâncias das suas classes de entidade, essas alterações são gravadas no `ChangeTracker` e então gravadas no banco de dados quando você chama `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="5e34a-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="5e34a-105">O provedor de banco de dados é responsável por converter as alterações em operações específicas do banco de dados (por exemplo, comandos `INSERT`, `UPDATE` e `DELETE` de um banco de dados relacional).</span><span class="sxs-lookup"><span data-stu-id="5e34a-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
