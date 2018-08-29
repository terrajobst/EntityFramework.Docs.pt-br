---
title: Índices – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993211"
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Um índice em um banco de dados relacional é mapeado para o mesmo conceito de um índice no núcleo do Entity Framework.

## <a name="conventions"></a>Convenções

Por convenção, os índices são nomeados `IX_<type name>_<property name>`. Para índices compostos `<property name>` se tornará uma lista de sublinhado separado de nomes de propriedade.

## <a name="data-annotations"></a>Anotações de dados

Índices não podem ser configurados usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome de um índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Você também pode especificar um filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Ao usar o provedor do SQL Server EF acrescenta um 'IS NOT NULL' Filtrar para todas as colunas que permitem valor nulas que fazem parte de um índice exclusivo. Para substituir essa convenção, você pode fornecer um `null` valor.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
