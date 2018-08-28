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
# <a name="data-seeding"></a>Propagação de dados

> [!NOTE]  
> Este recurso é novo no EF Core 2.1.

Permite a propagação de dados para fornecer os dados iniciais para popular um banco de dados. Diferentemente do EF6 no EF Core, a propagação de dados está associada um tipo de entidade como parte da configuração do modelo. Em seguida, o EF Core [migrações](xref:core/managing-schemas/migrations/index) pode calcular automaticamente o que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.

Por exemplo, você pode usar isso para configurar dados de semente para uma `Blog` em `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para adicionar entidades que têm uma relação de valores de chave estrangeira precisa ser especificado. Com frequência as propriedades de chave estrangeiras estão em estado de sombra, portanto ser capaz de definir os valores de uma classe anônima deve ser usado:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Após a adição de entidades, é recomendável usar [migrações](xref:core/managing-schemas/migrations/index) para aplicar as alterações. 

Como alternativa, você pode usar `context.Database.EnsureCreated()` para criar um novo banco de dados que contém os dados de semente, por exemplo, para um banco de dados de teste ou ao usar o provedor na memória. Observe que, se o banco de dados já existe, `EnsureCreated()` não atualizará o esquema nem os dados de propagação do banco de dados.
