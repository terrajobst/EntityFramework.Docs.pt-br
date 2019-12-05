---
title: Provedor de banco de dados Microsoft SQL Server-opções de banco de dados SQL do Azure-EF Core
description: Como especificar a camada de serviço e o nível de desempenho para o banco de dados SQL do Azure com o provedor de banco de dados SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824792"
---
# <a name="specifying-azure-sql-database-options"></a>Especificando opções do banco de dados SQL do Azure

>[!NOTE]
> Essa API é nova no EF Core 3,1.

O banco de dados SQL do Azure fornece [uma variedade de opções de preços](https://azure.microsoft.com/pricing/details/sql-database/single/) que geralmente são configuradas por meio do portal do Azure. No entanto, se você estiver gerenciando o esquema usando [EF Core migrações](xref:core/managing-schemas/migrations/index) , poderá especificar as opções desejadas no próprio modelo.

Você pode especificar a camada de serviço do banco de dados (edição) usando [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

Você pode especificar o tamanho máximo do banco de dados usando [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

Você pode especificar o nível de desempenho do banco de dados (SERVICE_OBJECTIVE) usando [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) para configurar o pool elástico, já que o valor não é um literal de cadeia de caracteres:

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> Você pode encontrar todos os valores com suporte na [documentação do ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).