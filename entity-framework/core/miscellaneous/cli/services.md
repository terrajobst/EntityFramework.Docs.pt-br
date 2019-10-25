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
# <a name="design-time-services"></a>Serviços de tempo de design

Alguns serviços usados pelas ferramentas só são usados em tempo de design. Esses serviços são gerenciados separadamente dos serviços de tempo de execução do EF Core para impedir que eles sejam implantados com seu aplicativo. Para substituir um desses serviços (por exemplo, o serviço para gerar arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
