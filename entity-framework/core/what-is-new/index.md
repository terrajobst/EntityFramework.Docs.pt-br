---
title: Lançamentos e planejamento do EF Core
author: ajcvickers
ms.date: 01/14/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 8d74c24021fd62c5c5d944eaf3973b344fdb1e9c
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124399"
---
# <a name="ef-core-releases-and-planning"></a>Lançamentos e planejamento do EF Core

## <a name="stable-releases"></a>Versões estáveis

| Versão | Estrutura de destino | Com suporte até | Links
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.1) | .NET Standard 2.0 | 3 de dezembro de 2022 (LTS) | [Comunicado](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| [EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.1) | .NET Standard 2.1 | 3 de março de 2020 | [Comunicado](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Alterações de falha](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Expirado em 23 de dezembro de 2019 | [Comunicado](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 de agosto de 2021 (LTS) | [Comunicado](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Expirado em 1º de outubro de 2018 | [Comunicado](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Expirado em 27 de junho de 2019 | [Comunicado](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Expirado em 27 de junho de 2019 | [Comunicado](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Confira [plataformas compatíveis](../platforms/index.md) para obter informações sobre as plataformas específicas compatíveis com cada versão EF Core.

Confira a [política de suporte do .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) para obter informações sobre a expiração de suporte e versões de LTS (suporte a longo prazo).

## <a name="guidance-on-updating-to-new-releases"></a>Diretrizes sobre como atualizar para novas versões

* As versões compatíveis recebem a aplicação de patches para segurança e correção de outros bugs críticos. Sempre use o patch mais recente de uma determinada versão. Por exemplo, para o EF Core 2.1, use o patch 2.1.14.
* As atualizações de versão principal (por exemplo, do EF Core 2 para a EF Core 3) geralmente têm alterações de falha. Um teste completo é recomendado ao atualizar entre versões principais. Use os links de alterações de falha acima para obter diretrizes sobre como lidar com alterações de falha.
* As atualizações de versão secundárias geralmente não contêm alterações de falha. No entanto, ainda é aconselhado realizar os testes completos, já que os novos recursos podem introduzir regressões.

## <a name="ef-core-50"></a>EF Core 5.0

As versões do EF Core se alinham com o [agendamento de envio do .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md). A próxima versão estável planejada é o **EF Core 5.0**, agendada para novembro de 2020.

Um [plano de alto nível para o EF Core 5.0](ef-core-5.0/plan.md) foi criado seguindo o [processo de planejamento de versão](release-planning.md) documentado.

Seus comentários sobre o planejamento são importantes. A melhor maneira de indicar a importância de um problema é votar (polegar para cima) nesse problema no GitHub. Esses dados serão então alimentados no processo de planejamento para a próxima versão.

### <a name="get-it-now"></a>Obtenha-o agora mesmo!

Os pacotes do EF Core 5.0 estão **disponíveis agora** como [builds diários](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

Usar os builds diários é uma ótima maneira de encontrar problemas e fornecer comentários a respeito o mais cedo possível. Quanto mais cedo obtivermos esses comentários, maior a probabilidade de podermos tomar atitudes em relação aos problemas correspondentes antes da próxima versão oficial. Trabalhamos muito para manter os builds diários em boas condições, executando mais de 55.000 testes por plataforma para cada build.

Os pacotes de versão prévia serão enviados para o NuGet posteriormente neste ano.
