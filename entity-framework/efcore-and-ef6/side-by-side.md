---
title: EF6 e EF Core – Como usá-los no mesmo aplicativo
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 6f95c02f4f24746605794832b1e26744fc554580
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995703"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Como usar o EF Core e o EF6 no mesmo aplicativo

É possível usar o EF Core e o EF6 no mesmo aplicativo ou biblioteca do .NET Framework instalando os dois pacotes NuGet.

Alguns tipos têm os mesmos nomes no EF Core e no EF6, e diferem apenas pelo namespace, o que pode complicar o uso do EF Core e do EF6 no mesmo arquivo de código. A ambiguidade pode ser removida facilmente usando diretivas de alias de namespace. Por exemplo:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Se você estiver realizando a portabilidade de um aplicativo existente que tenha vários modelos do EF, escolha seletivamente a portabilidade de alguns deles para o EF Core, e continue a usar o EF6 para os outros.
