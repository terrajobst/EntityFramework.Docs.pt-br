---
title: Restrições de chave estrangeira-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824583"
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
