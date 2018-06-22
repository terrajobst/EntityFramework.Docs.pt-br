---
title: EF Core e EF6 – Qual é o certo para você
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002815"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="a11c6-102">EF Core e EF6: Qual é o certo para você</span><span class="sxs-lookup"><span data-stu-id="a11c6-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="a11c6-103">As informações a seguir ajudarão você a escolher entre o Entity Framework Core e o Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a11c6-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="a11c6-104">Orientação para novos aplicativos</span><span class="sxs-lookup"><span data-stu-id="a11c6-104">Guidance for new applications</span></span>

<span data-ttu-id="a11c6-105">Considere usar o EF Core para novos aplicativos se quiser aproveitar todos os seus recursos e caso seu aplicativo não exija recursos ainda não implementados no EF Core.</span><span class="sxs-lookup"><span data-stu-id="a11c6-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="a11c6-106">Para utilizar o EF6, é necessário ter o .NET Framework 4.0 ou uma versão posterior. O EF6 só é compatível com o Windows, ou seja, não pode ser executado no .NET Core e não é compatível com outros sistemas operacionais. No entanto, ainda é uma opção viável para novos aplicativos, desde que essas restrições sejam aceitáveis e o aplicativo não exija novos recursos do EF Core que não estão disponíveis no EF6.</span><span class="sxs-lookup"><span data-stu-id="a11c6-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (i.e. it does not run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="a11c6-107">Examine [Comparação de Recursos](features.md) para ver se o EF Core pode ser uma escolha adequada para o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11c6-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="a11c6-108">Orientação para aplicativos EF6 existentes</span><span class="sxs-lookup"><span data-stu-id="a11c6-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="a11c6-109">Devido às mudanças fundamentais no EF Core, não recomendamos tentar mover um aplicativo EF6 para o EF Core, a menos que você tenha um motivo convincente para fazer essa alteração.</span><span class="sxs-lookup"><span data-stu-id="a11c6-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="a11c6-110">Se você quiser mudar para o EF Core a fim de usar os novos recursos, esteja ciente das limitações antes de começar.</span><span class="sxs-lookup"><span data-stu-id="a11c6-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="a11c6-111">Examine [Comparação de Recursos](features.md) para ver se o EF Core pode ser uma escolha adequada para o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11c6-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="a11c6-112">**Veja a mudança do EF6 para o EF Core como uma portabilidade em vez de uma atualização.**</span><span class="sxs-lookup"><span data-stu-id="a11c6-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="a11c6-113">Para saber mais, confira [Portabilidade do EF6 para o EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="a11c6-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
