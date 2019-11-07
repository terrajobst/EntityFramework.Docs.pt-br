---
title: Restrições de chave estrangeira-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655991"
---
# <a name="foreign-key-constraints"></a>Restrições de chave estrangeira

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma restrição FOREIGN KEY é introduzida para cada relação no modelo.

## <a name="conventions"></a>Convenções

Por convenção, as restrições FOREIGN KEY são nomeadas `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Para chaves estrangeiras compostas `<foreign key property name>` se torna uma lista separada por sublinhado de nomes de propriedade de chave estrangeira.

## <a name="data-annotations"></a>Anotações de dados

Nomes de restrição de chave estrangeira não podem ser configurados usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome da restrição de chave estrangeira para uma relação.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
