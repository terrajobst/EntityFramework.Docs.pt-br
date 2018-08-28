---
title: Selecionando a versão de tempo de execução do Entity Framework para modelos do EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998236"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="fc6c1-102">Selecionando a versão de tempo de execução do Entity Framework para modelos do EF Designer</span><span class="sxs-lookup"><span data-stu-id="fc6c1-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="fc6c1-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fc6c1-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="fc6c1-105">Começando com o EF6 a tela a seguir foi adicionada ao Designer de EF para que você possa selecionar a versão do tempo de execução que você deseja direcionar ao criar um modelo.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="fc6c1-106">A tela será exibida quando a versão mais recente do Entity Framework já não estiver instalada no projeto.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="fc6c1-107">Se a versão mais recente já está instalada. ele apenas será usado por padrão.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-107">If the latest version is already installed it will just be used by default.</span></span>

![Tela](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="fc6c1-109">Direcionamento EF6.x</span><span class="sxs-lookup"><span data-stu-id="fc6c1-109">Targeting EF6.x</span></span>

<span data-ttu-id="fc6c1-110">Você pode escolher o EF6 na tela 'Escolha sua versão' para adicionar o tempo de execução do EF6 ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="fc6c1-111">Depois que você adicionou o EF6, você irá parar vendo essa tela no projeto atual.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="fc6c1-112">EF6 será desabilitada se você já tiver uma versão mais antiga do EF instalado (já que não é possível direcionar várias versões do tempo de execução do mesmo projeto).</span><span class="sxs-lookup"><span data-stu-id="fc6c1-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="fc6c1-113">Se a opção do EF6 não está habilitada aqui, siga estas etapas para atualizar seu projeto para o EF6:</span><span class="sxs-lookup"><span data-stu-id="fc6c1-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="fc6c1-114">Clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="fc6c1-115">Selecione **atualizações**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-115">Select **Updates**</span></span>
3.  <span data-ttu-id="fc6c1-116">Selecione **EntityFramework** (Verifique se ele vai atualizá-lo para a versão desejada)</span><span class="sxs-lookup"><span data-stu-id="fc6c1-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="fc6c1-117">Clique em **atualização**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="fc6c1-118">Direcionamento EF5.x</span><span class="sxs-lookup"><span data-stu-id="fc6c1-118">Targeting EF5.x</span></span>

<span data-ttu-id="fc6c1-119">Você pode escolher o EF5 na tela 'Escolha sua versão' para adicionar o tempo de execução do EF5 para o seu projeto.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="fc6c1-120">Depois que você adicionou o EF5, você ainda verá a tela com a opção de EF6 desabilitada.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="fc6c1-121">Se você tiver uma versão EF4.x do tempo de execução já instalado, em seguida, você verá que a versão do EF listado na tela em vez de EF5.</span><span class="sxs-lookup"><span data-stu-id="fc6c1-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="fc6c1-122">Nessa situação, você pode atualizar para o EF5 usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="fc6c1-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="fc6c1-123">Selecione **ferramentas -&gt; Gerenciador de pacotes de biblioteca -&gt; Package Manager Console**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="fc6c1-124">Executar **Install-Package EntityFramework – versão 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="fc6c1-125">Direcionamento EF4.x</span><span class="sxs-lookup"><span data-stu-id="fc6c1-125">Targeting EF4.x</span></span>

<span data-ttu-id="fc6c1-126">Você pode instalar o tempo de execução EF4.x ao seu projeto usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="fc6c1-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="fc6c1-127">Selecione **ferramentas -&gt; Gerenciador de pacotes de biblioteca -&gt; Package Manager Console**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="fc6c1-128">Executar **Install-Package EntityFramework – versão 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="fc6c1-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
