---
title: Serviços de tempo de design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655831"
---
# <a name="design-time-services"></a><span data-ttu-id="81676-102">Serviços de tempo de design</span><span class="sxs-lookup"><span data-stu-id="81676-102">Design-time services</span></span>

<span data-ttu-id="81676-103">Alguns serviços usados pelas ferramentas só são usados em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="81676-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="81676-104">Esses serviços são gerenciados separadamente dos serviços de tempo de execução do EF Core para impedir que eles sejam implantados com seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="81676-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="81676-105">Para substituir um desses serviços (por exemplo, o serviço para gerar arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="81676-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
