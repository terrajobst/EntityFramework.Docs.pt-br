---
title: Serviços de tempo de design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811944"
---
# <a name="design-time-services"></a><span data-ttu-id="97d9c-102">Serviços de tempo de design</span><span class="sxs-lookup"><span data-stu-id="97d9c-102">Design-time services</span></span>

<span data-ttu-id="97d9c-103">Alguns serviços usados pelas ferramentas só são usados em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="97d9c-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="97d9c-104">Esses serviços são gerenciados separadamente dos serviços de tempo de execução do EF Core para impedir que eles sejam implantados com seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="97d9c-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="97d9c-105">Para substituir um desses serviços (por exemplo, o serviço para gerar arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="97d9c-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
