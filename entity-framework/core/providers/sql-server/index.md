---
title: Provedor de Banco de Dados do Microsoft SQL Server – EF Core
description: Documentação do provedor de banco de dados que permite que o Entity Framework Core seja usado com o Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413141"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="fd30e-103">Provedor de Banco de Dados EF Core do Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="fd30e-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="fd30e-104">Este provedor de banco de dados permite que o Entity Framework Core seja usado com o Microsoft SQL Server (incluindo o Banco de Dados SQL do Azure).</span><span class="sxs-lookup"><span data-stu-id="fd30e-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="fd30e-105">O provedor é mantido como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="fd30e-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="fd30e-106">Instalar o</span><span class="sxs-lookup"><span data-stu-id="fd30e-106">Install</span></span>

<span data-ttu-id="fd30e-107">Instale o [pacote NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="fd30e-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="fd30e-108">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="fd30e-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="fd30e-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fd30e-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="fd30e-110">Desde a versão 3.0.0, o provedor faz referência a Microsoft.Data.SqlClient (as versões anteriores dependem de System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="fd30e-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="fd30e-111">Se o seu projeto usa uma dependência direta no SqlClient, certifique-se de que ele faça referência ao pacote Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="fd30e-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="fd30e-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="fd30e-112">Supported Database Engines</span></span>

* <span data-ttu-id="fd30e-113">Microsoft SQL Server (2012 em diante)</span><span class="sxs-lookup"><span data-stu-id="fd30e-113">Microsoft SQL Server (2012 onwards)</span></span>
