---
title: Índices-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197019"
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Um índice em um banco de dados relacional é mapeado para o mesmo conceito de um índice no núcleo de Entity Framework.

## <a name="conventions"></a>Convenções

Por convenção, os índices são `IX_<type name>_<property name>`nomeados. Para índices `<property name>` compostos se torna uma lista de nomes de propriedade separados por sublinhado.

## <a name="data-annotations"></a>Anotações de dados

Os índices não podem ser configurados usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome de um índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Você também pode especificar um filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Ao usar o provedor de SQL Server EF, o adiciona um filtro ' IS NOT NULL ' para todas as colunas anuláveis que fazem parte de um índice exclusivo. Para substituir essa convenção, você pode fornecer `null` um valor.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Incluir colunas em índices de SQL Server

Você pode configurar [índices com colunas incluídas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para melhorar significativamente o desempenho da consulta quando todas as colunas na consulta são incluídas no índice como colunas de chave ou não chave.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
