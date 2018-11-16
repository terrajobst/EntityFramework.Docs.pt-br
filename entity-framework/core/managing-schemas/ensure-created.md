---
title: Criar e remover APIs – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688623"
---
# <a name="create-and-drop-apis"></a>Criar e remover APIs

Os métodos EnsureCreated e EnsureDeleted fornecem uma alternativa leve para [migrações](migrations/index.md) para gerenciar o esquema de banco de dados. Esses métodos são úteis em cenários quando os dados são transitórios e podem ser descartados quando o esquema é alterado. Por exemplo durante a criação de protótipos, testes ou para caches locais.

Alguns provedores (especialmente os não-relacional) não dão suporte a migrações. Para esses provedores, EnsureCreated costuma ser a maneira mais fácil para inicializar o esquema de banco de dados.

> [!WARNING]
> Migrações e EnsureCreated não funcionam bem juntos. Se você estiver usando as migrações, não use EnsureCreated para inicializar o esquema.

A transição de EnsureCreated para migrações não é uma experiência perfeita. A maneira mais simples de fazê-lo é remover o banco de dados e recriá-lo usando as migrações. Se você antecipar o uso de migrações no futuro, é melhor começar com as migrações em vez de usar EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

O método EnsureDeleted descartará o banco de dados se ele existir. Se você não tiver as permissões apropriadas, uma exceção é lançada.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated criará o banco de dados se ele não existe e inicializar o esquema de banco de dados. Se existirem quaisquer tabelas (incluindo tabelas para outra classe DbContext), o esquema não ser inicializado.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Versões assíncronas desses métodos também estão disponíveis.

## <a name="sql-script"></a>Script SQL

Para obter o SQL usado pelo EnsureCreated, você pode usar o método GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Várias classes DbContext

EnsureCreated só funciona quando não há tabelas estão presentes no banco de dados. Se necessário, você pode escrever sua própria verificação para ver se o esquema precisa ser inicializado e usar o serviço IRelationalDatabaseCreator subjacente para inicializar o esquema.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
