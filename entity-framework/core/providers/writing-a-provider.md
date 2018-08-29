---
title: Escrevendo um provedor de banco de dados – EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993660"
---
# <a name="writing-a-database-provider"></a>Escrevendo um provedor de banco de dados

Para obter informações sobre como escrever um provedor de banco de dados do Entity Framework Core, consulte [para que você deseja gravar um provedor do EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) pela [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Essas postagens não foram atualizadas desde o EF Core 1.1 e houve alterações significativas desde essa época [problema 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) está acompanhando as atualizações para essa documentação.

A base de código do EF Core é um software livre e contém vários provedores de banco de dados que podem ser usados como uma referência. Você pode encontrar o código-fonte em https://github.com/aspnet/EntityFrameworkCore. Ele também pode ser útil examinar o código para provedores de terceiros comumente usadas, como [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), e [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Em particular, esses projetos são configuradas para estender do e executar testes funcionais que publicamos no NuGet. Esse tipo de instalação é altamente recomendável.

## <a name="keeping-up-to-date-with-provider-changes"></a>Manter-se atualizado com alterações de provedor

Começando com o trabalho após a versão 2.1, criamos uma [log de alterações](provider-log.md) que talvez seja necessário que as alterações correspondentes no código do provedor. Isso serve para ajudar ao atualizar um provedor existente para trabalhar com uma nova versão do EF Core.

Antes de 2.1, usamos o [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) rótulos em nossos problemas do GitHub e solicitações de pull para uma finalidade similar. Iremos continiue para usar esses rótulos em problemas para dar uma indicação de quais itens de trabalho em uma determinada versão também podem exigir trabalho a ser feito em provedores. Um `providers-beware` rótulo normalmente significa que a implementação de um item de trabalho pode quebrar provedores, enquanto um `providers-fyi` rótulo normalmente significa que provedores não será interrompidos, mas o código pode precisar ser alterada de qualquer forma, por exemplo, para habilitar a nova funcionalidade.

## <a name="suggested-naming-of-third-party-providers"></a>Sugerido nomeação dos provedores de terceiros

Sugerimos usar a nomenclatura seguintes para pacotes do NuGet. Isso é consistente com os nomes de pacotes fornecidos pela equipe do EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Por exemplo:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
