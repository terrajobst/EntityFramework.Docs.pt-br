---
title: Provedor de Banco de Dados do Microsoft SQL Server – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812096"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="227c8-102">Provedor de Banco de Dados EF Core do Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="227c8-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="227c8-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Microsoft SQL Server (incluindo o SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="227c8-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="227c8-104">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="227c8-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="227c8-105">Instalar o</span><span class="sxs-lookup"><span data-stu-id="227c8-105">Install</span></span>

<span data-ttu-id="227c8-106">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="227c8-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="227c8-107">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="227c8-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="227c8-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="227c8-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="227c8-109">Desde a versão 3.0.0, o provedor faz referência a Microsoft.Data.SqlClient (as versões anteriores dependem de System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="227c8-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="227c8-110">Se o seu projeto usa uma dependência direta no SqlClient, verifique se ele faz referência ao pacote correto.</span><span class="sxs-lookup"><span data-stu-id="227c8-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="227c8-111">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="227c8-111">Supported Database Engines</span></span>

* <span data-ttu-id="227c8-112">Microsoft SQL Server (2012 em diante)</span><span class="sxs-lookup"><span data-stu-id="227c8-112">Microsoft SQL Server (2012 onwards)</span></span>
