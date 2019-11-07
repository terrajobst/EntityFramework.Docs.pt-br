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
# <a name="design-time-services"></a>Serviços de tempo de design

Alguns serviços usados pelas ferramentas só são usados em tempo de design. Esses serviços são gerenciados separadamente dos serviços de tempo de execução do EF Core para impedir que eles sejam implantados com seu aplicativo. Para substituir um desses serviços (por exemplo, o serviço para gerar arquivos de migração), adicione uma implementação de `IDesignTimeServices` ao seu projeto de inicialização.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
