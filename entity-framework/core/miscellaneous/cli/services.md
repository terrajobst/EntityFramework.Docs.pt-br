---
title: "Serviços de tempo de design - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a>Serviços de tempo de design
====================
Alguns serviços usados pelas ferramentas são usados somente em tempo de design. Esses serviços são gerenciados separadamente dos serviços de tempo de execução do núcleo do EF para impedir que está sendo implantado com seu aplicativo. Para substituir um desses serviços (por exemplo, o serviço para gerar os arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
