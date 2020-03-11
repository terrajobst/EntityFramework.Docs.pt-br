---
title: Criar e descartar APIs-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416866"
---
# <a name="create-and-drop-apis"></a>Criar e remover APIs

Os métodos EnsureCreated e EnsureDeleted fornecem uma alternativa leve às [migrações](migrations/index.md) para gerenciar o esquema de banco de dados. Esses métodos são úteis em cenários quando os dados são transitórios e podem ser descartados quando o esquema é alterado. Por exemplo, durante o protótipo, em testes ou para caches locais.

Alguns provedores (especialmente aqueles não relacionais) não oferecem suporte a migrações. Para esses provedores, a EnsureCreated geralmente é a maneira mais fácil de inicializar o esquema de banco de dados.

> [!WARNING]
> EnsureCreated e migrações não funcionam bem juntos. Se você estiver usando migrações, não use EnsureCreated para inicializar o esquema.

A transição de EnsureCreated para migrações não é uma experiência direta. A maneira mais simples de fazer isso é descartar o banco de dados e recriá-lo usando as migrações. Se você antecipar o uso de migrações no futuro, é melhor começar com as migrações em vez de usar EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

O método EnsureDeleted removerá o banco de dados se ele existir. Se você não tiver as permissões apropriadas, uma exceção será lançada.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

O EnsureCreated criará o banco de dados se ele não existir e inicializará o esquema de banco de dados. Se existir alguma tabela (incluindo tabelas para outra classe DbContext), o esquema não será inicializado.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> As versões assíncronas desses métodos também estão disponíveis.

## <a name="sql-script"></a>Script SQL

Para obter o SQL usado pelo EnsureCreated, você pode usar o método GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Várias classes DbContext

EnsureCreated só funciona quando nenhuma tabela está presente no banco de dados. Se necessário, você pode escrever seu próprio cheque para ver se o esquema precisa ser inicializado e usar o serviço IRelationalDatabaseCreator subjacente para inicializar o esquema.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
