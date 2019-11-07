---
title: Provedor de Azure Cosmos DB-limitações-EF Core
description: As limitações do provedor de Azure Cosmos DB de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655984"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Limitações do provedor de Azure Cosmos DB EF Core

O provedor Cosmos tem uma série de limitações. Muitas dessas limitações são um resultado de limitações no mecanismo de banco de dados Cosmos subjacente e não são específicas do EF. Mas a maioria simplesmente [não foi implementada ainda](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Limitações temporárias

- Mesmo se houver apenas um tipo de entidade sem herança mapeada para um contêiner, ele ainda terá uma propriedade discriminadora.
- Tipos de entidade com chaves de partição não funcionam corretamente em alguns cenários
- Não há suporte para chamadas de `Include`
- Não há suporte para chamadas de `Join`

## <a name="azure-cosmos-db-sdk-limitations"></a>Limitações do SDK do Azure Cosmos DB

- Somente métodos assíncronos são fornecidos

> [!WARNING]
> Como não há nenhuma versão de sincronização dos métodos de nível baixo EF Core depende, a funcionalidade correspondente é implementada no momento chamando `.Wait()` no `Task`retornado. Isso significa que usar métodos como `SaveChanges`ou `ToList` em vez de suas contrapartes assíncronas pode levar a um deadlock em seu aplicativo

## <a name="azure-cosmos-db-limitations"></a>Limitações de Azure Cosmos DB

Você pode ver a visão geral completa de [Azure Cosmos DB recursos com suporte](/azure/cosmos-db/modeling-data), essas são as diferenças mais notáveis em comparação com um banco de dados relacional:

- Não há suporte para transações iniciadas pelo cliente
- Algumas consultas entre partições não têm suporte ou muito mais lentamente, dependendo dos operadores envolvidos
