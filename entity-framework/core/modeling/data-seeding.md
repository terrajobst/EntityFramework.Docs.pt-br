---
title: "Propagação de dados - Core EF"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="e7d5a-102">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="e7d5a-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="e7d5a-103">Esse recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e7d5a-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e7d5a-104">Permite a propagação de dados para fornecer dados iniciais para popular um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7d5a-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="e7d5a-105">Ao contrário de EF6, no núcleo do EF, a propagação de dados está associada com um tipo de entidade como parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="e7d5a-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="e7d5a-106">Em seguida, migrações EF Core automaticamente podem calcular que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="e7d5a-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="e7d5a-107">Por exemplo, você pode usar isso para configurar dados de semente para uma `Blog` em `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="e7d5a-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="e7d5a-108">Para adicionar entidades que tenham uma relação de valores de chave estrangeira precisa ser especificado.</span><span class="sxs-lookup"><span data-stu-id="e7d5a-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="e7d5a-109">Com frequência as propriedades de chave estrangeiras estão em estado de sombra, portanto definir os valores de uma classe anônima deve ser usado:</span><span class="sxs-lookup"><span data-stu-id="e7d5a-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
