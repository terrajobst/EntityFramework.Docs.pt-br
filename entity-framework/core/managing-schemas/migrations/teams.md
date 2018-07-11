---
title: Migrações em ambientes de equipe – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228374"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="00c1f-102">Migrações em ambientes de equipe</span><span class="sxs-lookup"><span data-stu-id="00c1f-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="00c1f-103">Ao trabalhar com migrações em ambientes de equipe, preste atenção extra ao arquivo de instantâneo do modelo.</span><span class="sxs-lookup"><span data-stu-id="00c1f-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="00c1f-104">Esse arquivo pode informar se a migração do seu colega de equipe mescla corretamente com o seu ou se você precisar resolver um conflito, recriando a migração antes de compartilhá-lo.</span><span class="sxs-lookup"><span data-stu-id="00c1f-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="00c1f-105">Mesclando</span><span class="sxs-lookup"><span data-stu-id="00c1f-105">Merging</span></span>
-------
<span data-ttu-id="00c1f-106">Quando você mescla as migrações de seus colegas de equipe, você pode obter conflitos em seu arquivo de instantâneo do modelo.</span><span class="sxs-lookup"><span data-stu-id="00c1f-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="00c1f-107">Se ambas as alterações não estão relacionadas, a mesclagem é trivial e duas migrações podem coexistir.</span><span class="sxs-lookup"><span data-stu-id="00c1f-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="00c1f-108">Por exemplo, você pode obter um conflito de mesclagem em que a configuração do tipo de entidade customer tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="00c1f-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="00c1f-109">Uma vez que ambas as propriedades precisam existir no modelo final, conclua a mesclagem, adicionando ambas as propriedades.</span><span class="sxs-lookup"><span data-stu-id="00c1f-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="00c1f-110">Em muitos casos, o sistema de controle de versão pode mesclar essas alterações automaticamente para você.</span><span class="sxs-lookup"><span data-stu-id="00c1f-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="00c1f-111">Nesses casos, sua migração e a migração do seu colega de equipe são independentes uns dos outros.</span><span class="sxs-lookup"><span data-stu-id="00c1f-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="00c1f-112">Uma vez que qualquer um deles pode ser aplicado primeiro, você não precisa fazer nenhuma alteração adicional para a migração antes de compartilhá-lo com sua equipe.</span><span class="sxs-lookup"><span data-stu-id="00c1f-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="00c1f-113">A resolução de conflitos</span><span class="sxs-lookup"><span data-stu-id="00c1f-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="00c1f-114">Às vezes, você encontrar um conflito de true ao mesclar o modelo de instantâneo do modelo.</span><span class="sxs-lookup"><span data-stu-id="00c1f-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="00c1f-115">Por exemplo, você e seu colega de equipe talvez cada renomeou a mesma propriedade.</span><span class="sxs-lookup"><span data-stu-id="00c1f-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="00c1f-116">Se você encontrar esse tipo de conflito, resolvê-lo ao criar novamente sua migração.</span><span class="sxs-lookup"><span data-stu-id="00c1f-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="00c1f-117">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="00c1f-117">Follow these steps:</span></span>

1. <span data-ttu-id="00c1f-118">Anular a mesclagem e a reversão para seu diretório de trabalho antes da mesclagem</span><span class="sxs-lookup"><span data-stu-id="00c1f-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="00c1f-119">Remover sua migração (mas manter as alterações de modelo)</span><span class="sxs-lookup"><span data-stu-id="00c1f-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="00c1f-120">Mesclar alterações do seu colega de equipe em seu diretório de trabalho</span><span class="sxs-lookup"><span data-stu-id="00c1f-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="00c1f-121">Adicione novamente a migração</span><span class="sxs-lookup"><span data-stu-id="00c1f-121">Re-add your migration</span></span>

<span data-ttu-id="00c1f-122">Depois de fazer isso, duas migrações podem ser aplicadas na ordem correta.</span><span class="sxs-lookup"><span data-stu-id="00c1f-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="00c1f-123">Sua migração é aplicada primeiro, a coluna para a renomeação *Alias*, depois disso, sua migração renomeia-o para *nome de usuário*.</span><span class="sxs-lookup"><span data-stu-id="00c1f-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="00c1f-124">A migração pode ser compartilhada com segurança com o restante da equipe.</span><span class="sxs-lookup"><span data-stu-id="00c1f-124">Your migration can safely be shared with the rest of the team.</span></span>
