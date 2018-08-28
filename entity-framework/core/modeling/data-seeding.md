---
title: Propagação de dados – EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994473"
---
# <a name="data-seeding"></a><span data-ttu-id="42719-102">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="42719-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="42719-103">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="42719-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="42719-104">Permite a propagação de dados para fornecer os dados iniciais para popular um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="42719-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="42719-105">Diferentemente do EF6 no EF Core, a propagação de dados está associada um tipo de entidade como parte da configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="42719-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="42719-106">Em seguida, o EF Core [migrações](xref:core/managing-schemas/migrations/index) pode calcular automaticamente o que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.</span><span class="sxs-lookup"><span data-stu-id="42719-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="42719-107">Por exemplo, você pode usar isso para configurar dados de semente para uma `Blog` em `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="42719-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="42719-108">Para adicionar entidades que têm uma relação de valores de chave estrangeira precisa ser especificado.</span><span class="sxs-lookup"><span data-stu-id="42719-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="42719-109">Com frequência as propriedades de chave estrangeiras estão em estado de sombra, portanto ser capaz de definir os valores de uma classe anônima deve ser usado:</span><span class="sxs-lookup"><span data-stu-id="42719-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="42719-110">Após a adição de entidades, é recomendável usar [migrações](xref:core/managing-schemas/migrations/index) para aplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="42719-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="42719-111">Como alternativa, você pode usar `context.Database.EnsureCreated()` para criar um novo banco de dados que contém os dados de semente, por exemplo, para um banco de dados de teste ou ao usar o provedor na memória.</span><span class="sxs-lookup"><span data-stu-id="42719-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="42719-112">Observe que, se o banco de dados já existe, `EnsureCreated()` não atualizará o esquema nem os dados de propagação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="42719-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
