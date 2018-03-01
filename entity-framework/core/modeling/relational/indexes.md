---
title: "Índices - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Um índice em um banco de dados relacional é mapeado para o mesmo conceito que um índice no núcleo do Entity Framework.

## <a name="conventions"></a>Convenções

Por convenção, os índices são denominados `IX_<type name>_<property name>`. Para índices compostos `<property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade.

## <a name="data-annotations"></a>Anotações de dados

Índices não podem ser configurados usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome de um índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Você também pode especificar um filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Quando usar o provedor do SQL Server EF adiciona um 'IS NOT NULL' Filtrar todas as colunas anuláveis que fazem parte de um índice exclusivo. Para substituir essa convenção, você pode fornecer um `null` valor.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
