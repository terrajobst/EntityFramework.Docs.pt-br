---
title: Serviços de tempo de design - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053686"
---
<a name="design-time-services"></a><span data-ttu-id="0a06a-102">Serviços de tempo de design</span><span class="sxs-lookup"><span data-stu-id="0a06a-102">Design-time services</span></span>
====================
<span data-ttu-id="0a06a-103">Alguns serviços usados pelas ferramentas são usados somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="0a06a-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="0a06a-104">Esses serviços são gerenciados separadamente dos serviços de tempo de execução do núcleo do EF para impedir que está sendo implantado com seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0a06a-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="0a06a-105">Para substituir um desses serviços (por exemplo, o serviço para gerar os arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="0a06a-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
