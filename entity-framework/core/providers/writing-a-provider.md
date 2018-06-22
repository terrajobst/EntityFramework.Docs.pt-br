---
title: Escrevendo um provedor de banco de dados - Core EF
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678954"
---
# <a name="writing-a-database-provider"></a>Escrevendo um provedor de banco de dados

Para obter informações sobre como escrever um provedor de banco de dados do Entity Framework Core, consulte [para que você deseja gravar um provedor EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) por [Arthur Vickers](https://github.com/ajcvickers).

A base de código EF Core é o código-fonte aberto e contém vários provedores de banco de dados que podem ser usados como uma referência. Você pode encontrar o código-fonte em https://github.com/aspnet/EntityFrameworkCore.

## <a name="the-providers-beware-label"></a>O rótulo cuidado com provedores

Quando você começar a trabalhar em um provedor, observe o [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) rótulo em nossos GitHub problemas e solicitações pull. Usamos este rótulo para identificar as alterações que podem afetar os gravadores de provedor.

## <a name="suggested-naming-of-third-party-providers"></a>Sugerido nomeação de provedores de terceiros

Sugerimos usando a nomenclatura seguintes pacotes do NuGet. Isso é consistente com os nomes de pacotes entregues pela equipe do EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Por exemplo:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
