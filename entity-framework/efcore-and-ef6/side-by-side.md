---
title: EF6 e EF Core – Como usá-los no mesmo aplicativo
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949067"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="482ab-102">Como usar o EF Core e o EF6 no mesmo aplicativo</span><span class="sxs-lookup"><span data-stu-id="482ab-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="482ab-103">É possível usar o EF Core e o EF6 no mesmo aplicativo ou biblioteca do .NET Framework instalando os dois pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="482ab-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="482ab-104">Alguns tipos têm os mesmos nomes no EF Core e no EF6, e diferem apenas pelo namespace, o que pode complicar o uso do EF Core e do EF6 no mesmo arquivo de código.</span><span class="sxs-lookup"><span data-stu-id="482ab-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="482ab-105">A ambiguidade pode ser removida facilmente usando diretivas de alias de namespace.</span><span class="sxs-lookup"><span data-stu-id="482ab-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="482ab-106">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="482ab-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="482ab-107">Se você estiver realizando a portabilidade de um aplicativo existente que tenha vários modelos do EF, escolha seletivamente a portabilidade de alguns deles para o EF Core, e continue a usar o EF6 para os outros.</span><span class="sxs-lookup"><span data-stu-id="482ab-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
