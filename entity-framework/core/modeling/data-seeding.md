---
title: Propagação de dados - Core EF
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163194"
---
# <a name="data-seeding"></a><span data-ttu-id="ceaa8-102">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="ceaa8-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="ceaa8-103">Esse recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="ceaa8-104">Permite a propagação de dados para fornecer dados iniciais para popular um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="ceaa8-105">Ao contrário de EF6, no núcleo do EF, a propagação de dados está associada com um tipo de entidade como parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="ceaa8-106">Depois de EF Core [migrações](xref:core/managing-schemas/migrations/index) pode calcular automaticamente o que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="ceaa8-107">Por exemplo, você pode usar isso para configurar dados de semente para uma `Blog` em `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="ceaa8-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="ceaa8-108">Para adicionar entidades que tenham uma relação de valores de chave estrangeira precisa ser especificado.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="ceaa8-109">Com frequência as propriedades de chave estrangeiras estão em estado de sombra, portanto definir os valores de uma classe anônima deve ser usado:</span><span class="sxs-lookup"><span data-stu-id="ceaa8-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="ceaa8-110">Depois de adicionar entidades, é recomendável usar [migrações](xref:core/managing-schemas/migrations/index) para aplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="ceaa8-111">Como alternativa, você pode usar `context.Database.EnsureCreated()` para criar um novo banco de dados que contém os dados de semente, por exemplo, para um banco de dados de teste ou ao usar o provedor na memória.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="ceaa8-112">Observe que, se o banco de dados já existe, `EnsureCreated()` não atualizará o esquema nem os dados de propagação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ceaa8-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
