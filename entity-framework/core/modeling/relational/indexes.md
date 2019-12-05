---
title: Índices (banco de dados relacional)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824570"
---
# <a name="indexes-relational-database"></a>Índices (banco de dados relacional)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Um índice em um banco de dados relacional é mapeado para o mesmo conceito de um índice no núcleo de Entity Framework.

## <a name="conventions"></a>Convenções

Por convenção, os índices são nomeados `IX_<type name>_<property name>`. Para índices compostos `<property name>` se torna uma lista de nomes de propriedade separados por sublinhado.

## <a name="data-annotations"></a>Anotações de dados

Os índices não podem ser configurados usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome de um índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Você também pode especificar um filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Ao usar o provedor de SQL Server o EF adiciona um filtro de `'IS NOT NULL'` para todas as colunas anuláveis que fazem parte de um índice exclusivo. Para substituir essa convenção, você pode fornecer um valor `null`.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Incluir colunas em índices de SQL Server

Você pode configurar [índices com colunas incluídas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para melhorar significativamente o desempenho da consulta quando todas as colunas na consulta são incluídas no índice como colunas de chave ou não chave.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
