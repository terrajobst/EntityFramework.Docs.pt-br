---
title: Serviços de tempo de design – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997525"
---
<a name="design-time-services"></a><span data-ttu-id="f7500-102">Serviços de tempo de design</span><span class="sxs-lookup"><span data-stu-id="f7500-102">Design-time services</span></span>
====================
<span data-ttu-id="f7500-103">Alguns serviços usados pelas ferramentas são usados somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="f7500-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="f7500-104">Esses serviços são gerenciados separadamente dos serviços de tempo de execução do EF Core para impedir que elas sejam implantadas com seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f7500-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="f7500-105">Para substituir um desses serviços (por exemplo, o serviço para gerar arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="f7500-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
