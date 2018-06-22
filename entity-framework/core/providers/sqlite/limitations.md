---
title: Banco de dados SQLite provedor - limitações - EF Core
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 8a60ccfc61a5757df8ebedf257379d4d3dbffbf6
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29719479"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitações de provedor de banco de dados do SQLite EF Core

O provedor do SQLite tem várias limitações de migrações. A maioria dessas limitações é o resultado das limitações do mecanismo de banco de dados SQLite subjacente e não é específicas ao EF.

## <a name="modeling-limitations"></a>Limitações de modelagem

A biblioteca de relacional comuns (compartilhada por provedores de banco de dados relacional do Entity Framework) define APIs de modelagem conceitos que são comuns a maioria dos mecanismos de banco de dados relacional. Alguns desses conceitos não são suportados pelo provedor do SQLite.

* Esquemas
* Sequências

## <a name="migrations-limitations"></a>Limitações de migrações

O mecanismo de banco de dados SQLite não oferece suporte a um número de operações de esquema compatíveis com a maioria dos outros bancos de dados relacionais. Se você tentar aplicar uma das operações sem suporte para um banco de dados SQLite um `NotSupportedException` será lançada.

| Operação            | Com suporte? | Requer a versão |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (não operacional)  | 2.0              |
| DropSchema           | ✔ (não operacional)  | 2.0              |
| Inserir               | ✔          | 2.0              |
| Atualização               | ✔          | 2.0              |
| Excluir               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Solução alternativa de limitações de migrações

Você pode solucionar esse problema algumas dessas limitações manualmente escrevendo código em seu migrações para executar uma tabela de recompilar. Uma recriação de tabela envolve renomear a tabela existente, criando uma nova tabela, copiando dados para a nova tabela e descartar a tabela antiga. Você precisará usar o `Sql(string)` método para executar algumas dessas etapas.

Consulte [fazer outros tipos de alteração de esquema](http://sqlite.org/lang_altertable.html#otheralter) na documentação do SQLite para obter mais detalhes.

No futuro, EF pode dar suporte a algumas dessas operações usando a abordagem de recriação de tabela nos bastidores. Você pode [controlar esse recurso em nosso projeto GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
