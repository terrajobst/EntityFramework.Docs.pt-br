---
title: Selecionando Entity Framework versão de tempo de execução para modelos do EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418143"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="b3e1d-102">Selecionando Entity Framework versão de tempo de execução para modelos de designer do EF</span><span class="sxs-lookup"><span data-stu-id="b3e1d-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="b3e1d-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b3e1d-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b3e1d-105">A partir do EF6, a tela a seguir foi adicionada ao designer do EF para permitir que você selecione a versão do tempo de execução que você deseja direcionar ao criar um modelo.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="b3e1d-106">A tela será exibida quando a versão mais recente do Entity Framework ainda não estiver instalada no projeto.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="b3e1d-107">Se a versão mais recente já estiver instalada, ela será usada por padrão.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-107">If the latest version is already installed it will just be used by default.</span></span>

![Na](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="b3e1d-109">Direcionando EF6. x</span><span class="sxs-lookup"><span data-stu-id="b3e1d-109">Targeting EF6.x</span></span>

<span data-ttu-id="b3e1d-110">Você pode escolher EF6 na tela ' escolher sua versão ' para adicionar o tempo de execução do EF6 ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="b3e1d-111">Depois de adicionar o EF6, você deixará de ver esta tela no projeto atual.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="b3e1d-112">O EF6 será desabilitado se você já tiver uma versão mais antiga do EF instalada (já que não é possível direcionar várias versões do tempo de execução do mesmo projeto).</span><span class="sxs-lookup"><span data-stu-id="b3e1d-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="b3e1d-113">Se a opção EF6 não estiver habilitada aqui, siga estas etapas para atualizar seu projeto para EF6:</span><span class="sxs-lookup"><span data-stu-id="b3e1d-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="b3e1d-114">Clique com o botão direito do mouse em seu projeto no Gerenciador de Soluções e selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="b3e1d-115">Selecionar **atualizações**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-115">Select **Updates**</span></span>
3.  <span data-ttu-id="b3e1d-116">Selecione o **EntityFramework** (verifique se ele vai atualizá-lo para a versão desejada)</span><span class="sxs-lookup"><span data-stu-id="b3e1d-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="b3e1d-117">Clique em **Atualizar**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="b3e1d-118">Direcionando EF5. x</span><span class="sxs-lookup"><span data-stu-id="b3e1d-118">Targeting EF5.x</span></span>

<span data-ttu-id="b3e1d-119">Você pode escolher EF5 na tela ' escolher sua versão ' para adicionar o tempo de execução do EF5 ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="b3e1d-120">Depois de adicionar o EF5, você ainda verá a tela com a opção EF6 desabilitada.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="b3e1d-121">Se você tiver uma versão EF4. x do tempo de execução já instalada, verá essa versão do EF listada na tela em vez de EF5.</span><span class="sxs-lookup"><span data-stu-id="b3e1d-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="b3e1d-122">Nessa situação, você pode atualizar para o EF5 usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="b3e1d-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="b3e1d-123">Selecione **ferramentas –&gt; Gerenciador de pacotes de biblioteca-&gt; console do Gerenciador de pacotes**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="b3e1d-124">Executar **install-Package EntityFramework-Version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="b3e1d-125">Direcionando EF4. x</span><span class="sxs-lookup"><span data-stu-id="b3e1d-125">Targeting EF4.x</span></span>

<span data-ttu-id="b3e1d-126">Você pode instalar o tempo de execução do EF4. x em seu projeto usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="b3e1d-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="b3e1d-127">Selecione **ferramentas –&gt; Gerenciador de pacotes de biblioteca-&gt; console do Gerenciador de pacotes**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="b3e1d-128">Executar **install-Package EntityFramework-versão 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="b3e1d-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
