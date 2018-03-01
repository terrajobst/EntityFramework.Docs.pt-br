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
# <a name="data-seeding"></a>Propagação de dados

> [!NOTE]  
> Esse recurso é novo no EF Core 2.1.

Permite a propagação de dados para fornecer dados iniciais para popular um banco de dados. Ao contrário de EF6, no núcleo do EF, a propagação de dados está associada com um tipo de entidade como parte da configuração do modelo. Em seguida, migrações EF Core automaticamente podem calcular que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.

Por exemplo, você pode usar isso para configurar dados de semente para uma `Blog` em `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para adicionar entidades que tenham uma relação de valores de chave estrangeira precisa ser especificado. Com frequência as propriedades de chave estrangeiras estão em estado de sombra, portanto definir os valores de uma classe anônima deve ser usado:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
