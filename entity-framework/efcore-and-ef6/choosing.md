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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="7c083-102">EF Core e EF6: Qual é o certo para você</span><span class="sxs-lookup"><span data-stu-id="7c083-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="7c083-103">As informações a seguir ajudarão você a escolher entre o Entity Framework Core e o Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7c083-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="7c083-104">Orientação para novos aplicativos</span><span class="sxs-lookup"><span data-stu-id="7c083-104">Guidance for new applications</span></span>

<span data-ttu-id="7c083-105">Como o EF Core é um produto novo e ainda não tem alguns recursos críticos de O/RM, o EF6 ainda será a escolha mais adequada para muitos aplicativos.</span><span class="sxs-lookup"><span data-stu-id="7c083-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="7c083-106">**Estes são os tipos de aplicativos para os quais recomendaríamos o uso do EF Core:**</span><span class="sxs-lookup"><span data-stu-id="7c083-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="7c083-107">Novos aplicativos que não exigem recursos que ainda não foram implementados no EF Core.</span><span class="sxs-lookup"><span data-stu-id="7c083-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="7c083-108">Examine [Comparação de Recursos](features.md) para ver se o EF Core pode ser uma escolha adequada para o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c083-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="7c083-109">Aplicativos direcionados ao .NET Core, como UWP (Plataforma Universal do Windows) e aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c083-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="7c083-110">Esses aplicativos não podem usar EF6, pois ele exige o .NET Framework (ou seja, o .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="7c083-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="7c083-111">Orientação para aplicativos EF6 existentes</span><span class="sxs-lookup"><span data-stu-id="7c083-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="7c083-112">Devido às mudanças fundamentais no EF Core, não recomendamos tentar mover um aplicativo EF6 para o EF Core, a menos que você tenha um motivo convincente para fazer essa alteração.</span><span class="sxs-lookup"><span data-stu-id="7c083-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="7c083-113">Se você quiser mudar para o EF Core a fim de usar os novos recursos, esteja ciente das limitações antes de começar.</span><span class="sxs-lookup"><span data-stu-id="7c083-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="7c083-114">Examine [Comparação de Recursos](features.md) para ver se o EF Core pode ser uma escolha adequada para o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c083-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="7c083-115">**Veja a mudança do EF6 para o EF Core como uma portabilidade em vez de uma atualização.**</span><span class="sxs-lookup"><span data-stu-id="7c083-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="7c083-116">Para saber mais, confira [Portabilidade do EF6 para o EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="7c083-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
