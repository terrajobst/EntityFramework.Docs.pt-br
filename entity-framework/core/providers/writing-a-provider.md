---
title: Gravando um provedor de banco de dados-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d52a8581772cc5405e94966fa7ebdff4128c252
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654771"
---
# <a name="writing-a-database-provider"></a>Escrever um provedor de dados

Para obter informações sobre como escrever um provedor de banco de dados Entity Framework Core, consulte para [escrever um provedor de EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) de [Arthur Vicker](https://github.com/ajcvickers).

> [!NOTE]
> Essas postagens não foram atualizadas desde EF Core 1,1 e houve alterações significativas desde que o [problema 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) está acompanhando as atualizações desta documentação.

O EF Core codebase é código aberto e contém vários provedores de banco de dados que podem ser usados como referência. Você pode encontrar o código-fonte em <https://github.com/aspnet/EntityFrameworkCore>. Também pode ser útil examinar o código para provedores de terceiros comumente usados, como [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)e [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Em particular, esses projetos são configurados para estender e executar testes funcionais que publicamos no NuGet. Esse tipo de configuração é altamente recomendável.

## <a name="keeping-up-to-date-with-provider-changes"></a>Mantendo-se atualizado com as alterações do provedor

Começando com o trabalho após a versão 2,1, criamos um [log de alterações](provider-log.md) que podem precisar de alterações correspondentes no código do provedor. Isso é destinado a ajudar ao atualizar um provedor existente para trabalhar com uma nova versão do EF Core.

Antes de 2,1, usamos os rótulos [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) em nossos problemas do GitHub e solicitações pull para uma finalidade semelhante. Vamos continiue usar esses rótulos em problemas para dar uma indicação de quais itens de trabalho em uma determinada versão também podem exigir que o trabalho seja feito em provedores. Um rótulo de `providers-beware` normalmente significa que a implementação de um item de trabalho pode interromper provedores, enquanto um rótulo de `providers-fyi` normalmente significa que os provedores não serão interrompidos, mas talvez o código precise ser alterado de qualquer forma, por exemplo, para habilitar a nova funcionalidade.

## <a name="suggested-naming-of-third-party-providers"></a>Nomenclatura sugerida de provedores de terceiros

Sugerimos o uso da seguinte nomenclatura para pacotes NuGet. Isso é consistente com os nomes dos pacotes fornecidos pela equipe de EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Por exemplo:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
