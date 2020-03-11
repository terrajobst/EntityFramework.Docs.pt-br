---
title: Serviços de tempo de design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416724"
---
# <a name="design-time-services"></a><span data-ttu-id="0a247-102">Serviços de tempo de design</span><span class="sxs-lookup"><span data-stu-id="0a247-102">Design-time services</span></span>

<span data-ttu-id="0a247-103">Alguns serviços usados pelas ferramentas só são usados em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="0a247-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="0a247-104">Esses serviços são gerenciados separadamente dos serviços de tempo de execução do EF Core para impedir que eles sejam implantados com seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a247-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="0a247-105">Para substituir um desses serviços (por exemplo, o serviço para gerar arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="0a247-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
