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
---
# <a name="data-seeding"></a>Propagação de dados

> [!NOTE]  
> Esse recurso é novo no EF Core 2.1.

Permite a propagação de dados para fornecer dados iniciais para popular um banco de dados. Ao contrário de EF6, no núcleo do EF, a propagação de dados está associada com um tipo de entidade como parte da configuração do modelo. Depois de EF Core [migrações](xref:core/managing-schemas/migrations/index) pode calcular automaticamente o que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.

Por exemplo, você pode usar isso para configurar dados de semente para uma `Blog` em `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para adicionar entidades que tenham uma relação de valores de chave estrangeira precisa ser especificado. Com frequência as propriedades de chave estrangeiras estão em estado de sombra, portanto definir os valores de uma classe anônima deve ser usado:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Depois de adicionar entidades, é recomendável usar [migrações](xref:core/managing-schemas/migrations/index) para aplicar as alterações. 

Como alternativa, você pode usar `context.Database.EnsureCreated()` para criar um novo banco de dados que contém os dados de semente, por exemplo, para um banco de dados de teste ou ao usar o provedor na memória. Observe que, se o banco de dados já existe, `EnsureCreated()` não atualizará o esquema nem os dados de propagação do banco de dados.
