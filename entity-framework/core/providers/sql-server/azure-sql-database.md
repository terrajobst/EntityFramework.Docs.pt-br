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
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="7d39e-103">Especificando opções do banco de dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="7d39e-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="7d39e-104">Essa API é nova no EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="7d39e-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="7d39e-105">O banco de dados SQL do Azure fornece [uma variedade de opções de preços](https://azure.microsoft.com/pricing/details/sql-database/single/) que geralmente são configuradas por meio do portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="7d39e-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="7d39e-106">No entanto, se você estiver gerenciando o esquema usando [EF Core migrações](xref:core/managing-schemas/migrations/index) , poderá especificar as opções desejadas no próprio modelo.</span><span class="sxs-lookup"><span data-stu-id="7d39e-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="7d39e-107">Você pode especificar a camada de serviço do banco de dados (edição) usando [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="7d39e-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="7d39e-108">Você pode especificar o tamanho máximo do banco de dados usando [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="7d39e-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="7d39e-109">Você pode especificar o nível de desempenho do banco de dados (SERVICE_OBJECTIVE) usando [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="7d39e-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="7d39e-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) para configurar o pool elástico, já que o valor não é um literal de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="7d39e-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="7d39e-111">Você pode encontrar todos os valores com suporte na [documentação do ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span><span class="sxs-lookup"><span data-stu-id="7d39e-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>