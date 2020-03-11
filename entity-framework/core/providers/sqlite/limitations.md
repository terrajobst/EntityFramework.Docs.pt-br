---
title: Provedor de banco de dados SQLite-limitações-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417770"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>O SQLite EF Core limitações do provedor de banco de dados

O provedor SQLite tem várias limitações de migrações. A maioria dessas limitações é um resultado de limitações no mecanismo de banco de dados SQLite subjacente e não são específicas do EF.

## <a name="modeling-limitations"></a>Limitações de modelagem

A biblioteca relacional comum (compartilhada por Entity Framework provedores de banco de dados relacional) define APIs para conceitos de modelagem que são comuns à maioria dos mecanismos de banco de dados relacional. Alguns desses conceitos não são suportados pelo provedor SQLite.

* Esquemas
* Sequências
* Colunas computadas

## <a name="query-limitations"></a>Limitações de consulta

O SQLite não dá suporte nativo aos seguintes tipos de dados. EF Core pode ler e gravar valores desses tipos, e também há suporte para a consulta de igualdade (`where e.Property == value`). Outras operações, no entanto, como comparação e ordenação, exigirão avaliação no cliente.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

Em vez de `DateTimeOffset`, recomendamos o uso de valores DateTime. Ao lidar com vários fusos horários, é recomendável converter os valores em UTC antes de salvar e, em seguida, fazer a conversão de volta para o fuso horário apropriado.

O tipo de `Decimal` fornece um alto nível de precisão. No entanto, se você não precisar desse nível de precisão, é recomendável usar Double em vez disso. Você pode usar um [conversor de valor](../../modeling/value-conversions.md) para continuar usando decimal em suas classes.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Limitações de migrações

O mecanismo de banco de dados SQLite não oferece suporte a várias operações de esquema com suporte pela maioria de outros bancos de dados relacionais. Se você tentar aplicar uma das operações sem suporte a um banco de dados SQLite, uma `NotSupportedException` será lançada.

| Operação            | Porta? | Requer versão |
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
| Renomear          | ✔          | 1.0              |
| EnsureSchema         | ✔ (não op)  | 2,0              |
| DropSchema           | ✔ (não op)  | 2,0              |
| Insert               | ✔          | 2,0              |
| Atualizar               | ✔          | 2,0              |
| Excluir               | ✔          | 2,0              |

## <a name="migrations-limitations-workaround"></a>Solução alternativa para limitações de migrações

Você pode solucionar algumas dessas limitações, escrevendo manualmente o código em suas migrações para executar uma recompilação de tabela. Uma recriação de tabela envolve a renomeação da tabela existente, criando uma nova tabela, copiando dados para a nova tabela e removendo a tabela antiga. Você precisará usar o método `Sql(string)` para executar algumas dessas etapas.

Confira como [fazer outros tipos de alterações de esquema de tabela](https://sqlite.org/lang_altertable.html#otheralter) na documentação do SQLite para obter mais detalhes.

No futuro, o EF pode dar suporte a algumas dessas operações usando a abordagem de recriação de tabela nos bastidores. Você pode [acompanhar esse recurso em nosso projeto GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
