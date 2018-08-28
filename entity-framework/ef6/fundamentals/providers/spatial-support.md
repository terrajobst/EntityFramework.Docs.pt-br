---
title: Suporte do provedor para tipos espaciais – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 07eeecb5f5e3e3eab8548c4c7c0ed55c5ffb4f31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998281"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="e501d-102">Suporte do provedor para tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="e501d-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="e501d-103">Entity Framework dá suporte ao trabalho com dados espaciais pelas classes DbGeography ou DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="e501d-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="e501d-104">Essas classes dependem da funcionalidade de banco de dados específicos oferecida pelo provedor de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e501d-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="e501d-105">Nem todos os provedores dão suporte a dados espaciais e os que podem ter os pré-requisitos adicionais, como a instalação de assemblies de tipo espacial.</span><span class="sxs-lookup"><span data-stu-id="e501d-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="e501d-106">Para obter mais informações sobre o suporte do provedor para tipos espaciais são fornecidas abaixo.</span><span class="sxs-lookup"><span data-stu-id="e501d-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="e501d-107">Informações adicionais sobre como usar tipos espaciais em um aplicativo podem ser encontradas em duas instruções passo a passo, um para o Code First, outro para Model First ou Database First:</span><span class="sxs-lookup"><span data-stu-id="e501d-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="e501d-108">Tipos de dados espaciais no código pela primeira vez</span><span class="sxs-lookup"><span data-stu-id="e501d-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="e501d-109">Tipos de dados espaciais no EF Designer</span><span class="sxs-lookup"><span data-stu-id="e501d-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="e501d-110">Versões EF que dão suporte a tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="e501d-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="e501d-111">Suporte para tipos espaciais foi introduzido no EF5.</span><span class="sxs-lookup"><span data-stu-id="e501d-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="e501d-112">No entanto, no EF5 tipos espaciais só têm suporte quando o aplicativo tem como alvo e é executado no .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e501d-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="e501d-113">Começando com o EF6 tipos espaciais têm suporte para aplicativos destinados ao .NET 4 e .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e501d-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="e501d-114">Provedores do EF que dão suporte a tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="e501d-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="e501d-115">EF5</span><span class="sxs-lookup"><span data-stu-id="e501d-115">EF5</span></span>  

<span data-ttu-id="e501d-116">Os provedores de Entity Framework para EF5 que estamos cientes de que deem suporte a tipos espaciais são:</span><span class="sxs-lookup"><span data-stu-id="e501d-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="e501d-117">Provedor do Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e501d-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="e501d-118">Esse provedor é fornecido como parte do EF5.</span><span class="sxs-lookup"><span data-stu-id="e501d-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="e501d-119">Esse provedor depende de algumas bibliotecas de nível baixo adicionais que talvez precise ser instalado — consulte abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="e501d-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="e501d-120">Devart dotConnect para Oracle</span><span class="sxs-lookup"><span data-stu-id="e501d-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="e501d-121">Esse é um provedor de terceiros do Devart.</span><span class="sxs-lookup"><span data-stu-id="e501d-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="e501d-122">Se você souber de um provedor do EF5 que dá suporte a tipos espaciais, em seguida, por favor, entrar em contato e ficaremos felizes de adicioná-lo a essa lista.</span><span class="sxs-lookup"><span data-stu-id="e501d-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="e501d-123">EF6</span><span class="sxs-lookup"><span data-stu-id="e501d-123">EF6</span></span>  

<span data-ttu-id="e501d-124">Os provedores de Entity Framework para o EF6 que estamos cientes de que deem suporte a tipos espaciais são:</span><span class="sxs-lookup"><span data-stu-id="e501d-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="e501d-125">Provedor do Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e501d-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="e501d-126">Esse provedor é fornecido como parte do EF6.</span><span class="sxs-lookup"><span data-stu-id="e501d-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="e501d-127">Esse provedor depende de algumas bibliotecas de nível baixo adicionais que talvez precise ser instalado — consulte abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="e501d-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="e501d-128">Devart dotConnect para Oracle</span><span class="sxs-lookup"><span data-stu-id="e501d-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="e501d-129">Esse é um provedor de terceiros do Devart.</span><span class="sxs-lookup"><span data-stu-id="e501d-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="e501d-130">Se você souber de um provedor do EF6 que dá suporte a tipos espaciais, em seguida, por favor, entrar em contato e ficaremos felizes de adicioná-lo a essa lista.</span><span class="sxs-lookup"><span data-stu-id="e501d-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="e501d-131">Pré-requisitos para tipos espaciais com o Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e501d-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="e501d-132">Suporte espacial do SQL Server depende de baixo nível, tipos específicos do SQL Server SqlGeography e SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="e501d-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="e501d-133">Esses tipos de ativos no assembly Types e esse assembly não é enviado como parte do EF ou como parte do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e501d-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="e501d-134">Quando o Visual Studio está instalado ele geralmente também instalará uma versão do SQL Server, e isso inclui a instalação do Types.</span><span class="sxs-lookup"><span data-stu-id="e501d-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="e501d-135">Se o SQL Server não está instalado no computador em que você deseja usar tipos espaciais, ou se os tipos espaciais foram excluídos da instalação do SQL Server, você precisará instalá-los manualmente.</span><span class="sxs-lookup"><span data-stu-id="e501d-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="e501d-136">Os tipos podem ser instalados usando `SQLSysClrTypes.msi`, que faz parte do Feature Pack do Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e501d-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="e501d-137">Tipos espaciais são específicos da versão, do SQL Server, portanto, recomendamos [pesquisar "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) no Microsoft Download Center, em seguida, selecione e baixe a opção que corresponde à versão do SQL Server, você usará.</span><span class="sxs-lookup"><span data-stu-id="e501d-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
