---
title: Suporte do provedor para tipos espaciais-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181592"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="5f8d3-102">Suporte do provedor para tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="5f8d3-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="5f8d3-103">Entity Framework dá suporte ao trabalho com dados espaciais por meio das classes DbGeography ou DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="5f8d3-104">Essas classes dependem da funcionalidade específica do banco de dados oferecida pelo provedor de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="5f8d3-105">Nem todos os provedores dão suporte a dados espaciais e aqueles que podem ter pré-requisitos adicionais, como a instalação de assemblies de tipo espacial.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="5f8d3-106">Mais informações sobre o suporte do provedor para tipos espaciais são fornecidas abaixo.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="5f8d3-107">Informações adicionais sobre como usar tipos espaciais em um aplicativo podem ser encontradas em dois passo a passos, um para Code First, o outro para Database First ou Model First:</span><span class="sxs-lookup"><span data-stu-id="5f8d3-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="5f8d3-108">Tipos de dados espaciais no Code First</span><span class="sxs-lookup"><span data-stu-id="5f8d3-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="5f8d3-109">Tipos de dados espaciais no designer do EF</span><span class="sxs-lookup"><span data-stu-id="5f8d3-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="5f8d3-110">Versões do EF que dão suporte a tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="5f8d3-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="5f8d3-111">O suporte para tipos espaciais foi introduzido em EF5.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="5f8d3-112">No entanto, em tipos espaciais EF5 só há suporte quando o aplicativo é direcionado e executado no .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="5f8d3-113">A partir do EF6, os tipos espaciais têm suporte para aplicativos destinados ao .NET 4 e ao .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="5f8d3-114">Provedores de EF que dão suporte a tipos espaciais</span><span class="sxs-lookup"><span data-stu-id="5f8d3-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="5f8d3-115">EF5</span><span class="sxs-lookup"><span data-stu-id="5f8d3-115">EF5</span></span>  

<span data-ttu-id="5f8d3-116">Os provedores de Entity Framework para EF5 que estamos cientes desse suporte são os tipos espaciais:</span><span class="sxs-lookup"><span data-stu-id="5f8d3-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="5f8d3-117">Provedor de Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5f8d3-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="5f8d3-118">Esse provedor é enviado como parte do EF5.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="5f8d3-119">Esse provedor depende de algumas bibliotecas de nível inferior adicionais que talvez precisem ser instaladas – consulte abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="5f8d3-120">Devart dotConnect para Oracle</span><span class="sxs-lookup"><span data-stu-id="5f8d3-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="5f8d3-121">Este é um provedor de terceiros da Devart.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="5f8d3-122">Se você souber de um provedor EF5 que dá suporte a tipos espaciais, entre em contato e teremos o prazer de adicioná-lo a essa lista.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="5f8d3-123">EF6</span><span class="sxs-lookup"><span data-stu-id="5f8d3-123">EF6</span></span>  

<span data-ttu-id="5f8d3-124">Os provedores de Entity Framework para EF6 que estamos cientes desse suporte são os tipos espaciais:</span><span class="sxs-lookup"><span data-stu-id="5f8d3-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="5f8d3-125">Provedor de Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5f8d3-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="5f8d3-126">Esse provedor é enviado como parte do EF6.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="5f8d3-127">Esse provedor depende de algumas bibliotecas de nível inferior adicionais que talvez precisem ser instaladas – consulte abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="5f8d3-128">Devart dotConnect para Oracle</span><span class="sxs-lookup"><span data-stu-id="5f8d3-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="5f8d3-129">Este é um provedor de terceiros da Devart.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="5f8d3-130">Se você souber de um provedor EF6 que dá suporte a tipos espaciais, entre em contato e teremos o prazer de adicioná-lo a essa lista.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="5f8d3-131">Pré-requisitos para tipos espaciais com Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5f8d3-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="5f8d3-132">SQL Server suporte espacial depende dos tipos de baixo nível SQL Server específicos, geography e SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="5f8d3-133">Esses tipos residem no assembly Microsoft. SqlServer. Types. dll, e esse assembly não é enviado como parte do EF ou como parte do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="5f8d3-134">Quando o Visual Studio é instalado, ele geralmente também instala uma versão do SQL Server, e isso incluirá a instalação do Microsoft. SqlServer. Types. dll.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="5f8d3-135">Se o SQL Server não estiver instalado no computador em que você deseja usar tipos espaciais, ou se os tipos espaciais tiverem sido excluídos da instalação do SQL Server, você precisará instalá-los manualmente.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="5f8d3-136">Os tipos podem ser instalados usando `SQLSysClrTypes.msi`, que é parte do Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="5f8d3-137">Os tipos espaciais são SQL Server específicos à versão, portanto, é recomendável [Pesquisar "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) no centro de download da Microsoft e, em seguida, selecionar e baixar a opção que corresponde à versão do SQL Server que você usará.</span><span class="sxs-lookup"><span data-stu-id="5f8d3-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
