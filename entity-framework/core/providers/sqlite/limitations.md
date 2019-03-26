---
title: Banco de dados SQLite provedor - limitações – EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: ce834d60b9ceb4c414f097f2d86254cc5edd958f
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419699"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitações do provedor de banco de dados SQLite EF Core

O provedor SQLite tem uma série de limitações de migrações. A maioria dessas limitações é o resultado das limitações no mecanismo de banco de dados subjacente do SQLite e não é específicas ao EF.

## <a name="modeling-limitations"></a>Limitações de modelagem

A biblioteca de relacional comuns (compartilhada pelos provedores de banco de dados relacional do Entity Framework) define as APIs de modelagem de conceitos que são comuns a maioria dos mecanismos de banco de dados relacional. Alguns desses conceitos não são suportados pelo provedor SQLite.

* Esquemas
* Sequências
* Colunas computadas

## <a name="migrations-limitations"></a>Limitações de migrações

O mecanismo de banco de dados SQLite não oferece suporte a um número de operações de esquema que são compatíveis com a maioria dos outros bancos de dados relacionais. Se você tentar aplicar uma das operações sem suporte a um banco de dados SQLite, um `NotSupportedException` será lançada.

| Operação            | Suporte? | Requer a versão |
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
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (não operacional)  | 2.0              |
| DropSchema           | ✔ (não operacional)  | 2.0              |
| Inserir               | ✔          | 2.0              |
| Atualização               | ✔          | 2.0              |
| Excluir               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Solução alternativa de limitações de migrações

É possível solucionar algumas dessas limitações manualmente escrevendo código em suas migrações para realizar uma tabela de recompilar. Uma recriação de tabela envolve a renomeação da tabela existente, criando uma nova tabela, copiando dados para a nova tabela e removendo a tabela antiga. Você precisará usar o `Sql(string)` método para executar algumas dessas etapas.

Ver [fazer outros tipos de alteração de esquema](http://sqlite.org/lang_altertable.html#otheralter) na documentação do SQLite para obter mais detalhes.

No futuro, o EF pode dar suporte a algumas dessas operações usando a abordagem de recriação de tabela nos bastidores. Você pode [controlar esse recurso em nosso projeto GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
