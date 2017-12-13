---
title: "EF Core e EF6 – Qual é o certo para você"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core e EF6: Qual é o certo para você

As informações a seguir ajudarão você a escolher entre o Entity Framework Core e o Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Orientação para novos aplicativos

Como o EF Core é um produto novo e ainda não tem alguns recursos críticos de O/RM, o EF6 ainda será a escolha mais adequada para muitos aplicativos.

**Estes são os tipos de aplicativos para os quais recomendaríamos o uso do EF Core:**

* Novos aplicativos que não exigem recursos que ainda não foram implementados no EF Core. Examine [Comparação de Recursos](features.md) para ver se o EF Core pode ser uma escolha adequada para o seu aplicativo.

* Aplicativos direcionados ao .NET Core, como UWP (Plataforma Universal do Windows) e aplicativos do ASP.NET Core. Esses aplicativos não podem usar EF6, pois ele exige o .NET Framework (ou seja, o .NET Framework 4.5).

## <a name="guidance-for-existing-ef6-applications"></a>Orientação para aplicativos EF6 existentes

Devido às mudanças fundamentais no EF Core, não recomendamos tentar mover um aplicativo EF6 para o EF Core, a menos que você tenha um motivo convincente para fazer essa alteração. Se você quiser mudar para o EF Core a fim de usar os novos recursos, esteja ciente das limitações antes de começar. Examine [Comparação de Recursos](features.md) para ver se o EF Core pode ser uma escolha adequada para o seu aplicativo.

**Veja a mudança do EF6 para o EF Core como uma portabilidade em vez de uma atualização.** Para saber mais, confira [Portabilidade do EF6 para o EF Core](porting/index.md).
