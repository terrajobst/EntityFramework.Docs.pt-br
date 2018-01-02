---
title: "Provedor de Banco de Dados Pomelo MyCat – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/27/2017
ms.assetid: ad798f02-a7a4-45c1-b0a9-8e92f5dc6ff0
ms.technology: entity-framework-core
uid: core/providers/my-cat/index
ms.openlocfilehash: e13da3ab47e56d1b869e445f2398eda6ea84a79f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="pomelo-mycat-ef-core-database-provider"></a><span data-ttu-id="6959a-102">Provedor de Banco de Dados do EF Core Pomelo MyCat</span><span class="sxs-lookup"><span data-stu-id="6959a-102">Pomelo MyCat EF Core Database Provider</span></span>

<span data-ttu-id="6959a-103">Este provedor de banco de dados permite que o Entity Framework Core seja usado com [MyCat](https://github.com/MyCATApache/Mycat-Server).</span><span class="sxs-lookup"><span data-stu-id="6959a-103">This database provider allows Entity Framework Core to be used with [MyCat](https://github.com/MyCATApache/Mycat-Server).</span></span> <span data-ttu-id="6959a-104">O provedor é mantido como parte do [Projeto da Pomelo Foundation](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span><span class="sxs-lookup"><span data-stu-id="6959a-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span></span>

> [!NOTE]  
> <span data-ttu-id="6959a-105">Este provedor não é mantido como parte do projeto do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6959a-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="6959a-106">Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.</span><span class="sxs-lookup"><span data-stu-id="6959a-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="6959a-107">Instalar o</span><span class="sxs-lookup"><span data-stu-id="6959a-107">Install</span></span>

<span data-ttu-id="6959a-108">Baixe e execute [a versão mais recente do site do projeto](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span><span class="sxs-lookup"><span data-stu-id="6959a-108">Download and run [the latest release from the project site](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span></span>

## <a name="get-started"></a><span data-ttu-id="6959a-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="6959a-109">Get Started</span></span>

<span data-ttu-id="6959a-110">Os recursos a seguir ajudará você a começar a usar esse provedor.</span><span class="sxs-lookup"><span data-stu-id="6959a-110">The following resources will help you get started with this provider.</span></span>
 * [<span data-ttu-id="6959a-111">Etapas da configuração</span><span class="sxs-lookup"><span data-stu-id="6959a-111">Setup steps</span></span>](https://github.com/aspnet/EntityFramework.Docs/issues/252)
 * [<span data-ttu-id="6959a-112">Vídeo de introdução</span><span class="sxs-lookup"><span data-stu-id="6959a-112">Getting started video</span></span>](https://www.youtube.com/watch?v=q0CXfFNtMZo)

## <a name="supported-database-engines"></a><span data-ttu-id="6959a-113">Mecanismos de banco de dados compatíveis</span><span class="sxs-lookup"><span data-stu-id="6959a-113">Supported Database Engines</span></span>

* <span data-ttu-id="6959a-114">MyCat</span><span class="sxs-lookup"><span data-stu-id="6959a-114">MyCat</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="6959a-115">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="6959a-115">Supported Platforms</span></span>

* <span data-ttu-id="6959a-116">.NET Framework (4.5.1 em diante)</span><span class="sxs-lookup"><span data-stu-id="6959a-116">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="6959a-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6959a-117">.NET Core</span></span>
* <span data-ttu-id="6959a-118">Mono (4.2.0 em diante)</span><span class="sxs-lookup"><span data-stu-id="6959a-118">Mono (4.2.0 onwards)</span></span>
