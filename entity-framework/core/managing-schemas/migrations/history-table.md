---
title: Tabela de histórico de migrações personalizadas-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655711"
---
# <a name="custom-migrations-history-table"></a>Tabela de histórico de migrações personalizadas

Por padrão, EF Core controla quais migrações foram aplicadas ao banco de dados, gravando-as em uma tabela chamada `__EFMigrationsHistory`. Por vários motivos, talvez você queira personalizar essa tabela para atender melhor às suas necessidades.

> [!IMPORTANT]
> Se você personalizar a tabela de histórico de migrações *depois* de aplicar as migrações, você será responsável por atualizar a tabela existente no banco de dados.

## <a name="schema-and-table-name"></a>Nome do esquema e da tabela

Você pode alterar o nome do esquema e da tabela usando o método `MigrationsHistoryTable()` em `OnConfiguring()` (ou `ConfigureServices()` em ASP.NET Core). Aqui está um exemplo usando o provedor de EF Core de SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Outras alterações

Para configurar aspectos adicionais da tabela, substitua e substitua o serviço de `IHistoryRepository` específico do provedor. Aqui está um exemplo de alteração do nome da coluna Migrationid para *ID* em SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` está dentro de um namespace interno e pode ser alterado em versões futuras.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
