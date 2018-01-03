---
title: "Provedor de Banco de Dados MySQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="527f9-102">Provedor de Banco de Dados EF Core MySQL</span><span class="sxs-lookup"><span data-stu-id="527f9-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="527f9-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com MySQL.</span><span class="sxs-lookup"><span data-stu-id="527f9-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="527f9-104">O provedor é mantido como parte do [projeto MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="527f9-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="527f9-105">Esse provedor é a versão de pré-lançamento.</span><span class="sxs-lookup"><span data-stu-id="527f9-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="527f9-106">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="527f9-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="527f9-107">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="527f9-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="527f9-108">Instalar o</span><span class="sxs-lookup"><span data-stu-id="527f9-108">Install</span></span>

<span data-ttu-id="527f9-109">Instale o [pacote NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="527f9-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="527f9-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="527f9-110">Get Started</span></span>

<span data-ttu-id="527f9-111">Consulte [Introdução ao provedor MySQL EF Core e Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="527f9-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="527f9-112">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="527f9-112">Supported Database Engines</span></span>

* <span data-ttu-id="527f9-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="527f9-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="527f9-114">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="527f9-114">Supported Platforms</span></span>

* <span data-ttu-id="527f9-115">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="527f9-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="527f9-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="527f9-116">.NET Core</span></span>

<span data-ttu-id="527f9-117">Não deixe de examinar a documentação do MySQL para obter informações de compatibilidade de versão [aqui](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) e [aqui](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="527f9-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
