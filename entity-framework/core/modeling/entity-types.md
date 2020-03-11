---
title: Tipos de entidade-EF Core
description: Como configurar e mapear tipos de entidade usando Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417229"
---
# <a name="entity-types"></a>Tipos de entidade

Incluir um DbSet de um tipo no seu contexto significa que ele está incluído no modelo de EF Core; Geralmente, nos referimos a um tipo como uma *entidade*. EF Core pode ler e gravar instâncias de entidade de/para o banco de dados e, se você estiver usando um banco de dados relacional, EF Core poderá criar tabelas para suas entidades por meio de migrações.

## <a name="including-types-in-the-model"></a>Incluindo tipos no modelo

Por convenção, os tipos expostos nas propriedades DbSet no seu contexto são incluídos no modelo como entidades. Os tipos de entidade que são especificados no método `OnModelCreating` também são incluídos, assim como os tipos que são encontrados por explorar recursivamente as propriedades de navegação de outros tipos de entidade descobertas.

No exemplo de código abaixo, todos os tipos são incluídos:

* `Blog` está incluído porque é exposto em uma propriedade DbSet no contexto.
* `Post` está incluído porque é descoberto por meio da propriedade de navegação `Blog.Posts`.
* `AuditEntry` porque ele está especificado em `OnModelCreating`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>Excluindo tipos do modelo

Se você não quiser que um tipo seja incluído no modelo, você pode excluí-lo:

### <a name="data-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a>Nome da tabela

Por convenção, cada tipo de entidade será configurado para mapear para uma tabela de banco de dados com o mesmo nome que a propriedade DbSet que expõe a entidade. Se não existir nenhum DbSet para a entidade especificada, o nome da classe será usado.

Você pode configurar manualmente o nome da tabela:

### <a name="data-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a>Esquema de tabela

Ao usar um banco de dados relacional, as tabelas são por convenção criadas no esquema padrão do banco de dados. Por exemplo, Microsoft SQL Server usará o esquema de `dbo` (o SQLite não oferece suporte a esquemas).

Você pode configurar tabelas a serem criadas em um esquema específico da seguinte maneira:

### <a name="data-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

Em vez de especificar o esquema para cada tabela, você também pode definir o esquema padrão no nível do modelo com a API Fluent:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

Observe que a configuração do esquema padrão também afetará outros objetos de banco de dados, como sequências.
