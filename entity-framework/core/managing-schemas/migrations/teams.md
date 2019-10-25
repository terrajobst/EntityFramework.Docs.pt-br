---
title: Migrações em ambientes de equipe-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: e6a1b86761a201cbcae34cced7e64f11df37a420
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811987"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="e7b55-102">Migrações em ambientes de equipe</span><span class="sxs-lookup"><span data-stu-id="e7b55-102">Migrations in Team Environments</span></span>

<span data-ttu-id="e7b55-103">Ao trabalhar com migrações em ambientes de equipe, preste atenção extra ao arquivo de instantâneo do modelo.</span><span class="sxs-lookup"><span data-stu-id="e7b55-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="e7b55-104">Esse arquivo pode informar se a migração de seu colega de equipe é mesclada corretamente com sua ou se você precisa resolver um conflito recriando a migração antes de compartilhá-la.</span><span class="sxs-lookup"><span data-stu-id="e7b55-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="e7b55-105">Mesclando</span><span class="sxs-lookup"><span data-stu-id="e7b55-105">Merging</span></span>

<span data-ttu-id="e7b55-106">Ao mesclar migrações de seus colegas de equipe, você poderá obter conflitos no arquivo de instantâneo do modelo.</span><span class="sxs-lookup"><span data-stu-id="e7b55-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="e7b55-107">Se ambas as alterações não estiverem relacionadas, a mesclagem será trivial e as duas migrações poderão coexistir.</span><span class="sxs-lookup"><span data-stu-id="e7b55-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="e7b55-108">Por exemplo, você pode obter um conflito de mesclagem na configuração do tipo de entidade Customer que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="e7b55-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="e7b55-109">Como essas duas propriedades precisam existir no modelo final, conclua a mesclagem adicionando ambas as propriedades.</span><span class="sxs-lookup"><span data-stu-id="e7b55-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="e7b55-110">Em muitos casos, seu sistema de controle de versão pode mesclar automaticamente essas alterações para você.</span><span class="sxs-lookup"><span data-stu-id="e7b55-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="e7b55-111">Nesses casos, a migração e a migração de seu colega de equipe são independentes umas das outras.</span><span class="sxs-lookup"><span data-stu-id="e7b55-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="e7b55-112">Como qualquer um deles pode ser aplicado primeiro, você não precisa fazer nenhuma alteração adicional na sua migração antes de compartilhá-la com sua equipe.</span><span class="sxs-lookup"><span data-stu-id="e7b55-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="e7b55-113">Resolvendo conflitos</span><span class="sxs-lookup"><span data-stu-id="e7b55-113">Resolving conflicts</span></span>

<span data-ttu-id="e7b55-114">Às vezes, você encontra um verdadeiro conflito ao mesclar o modelo de instantâneo do modelo.</span><span class="sxs-lookup"><span data-stu-id="e7b55-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="e7b55-115">Por exemplo, você e seu colega de equipe podem ter renomeado a mesma propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7b55-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="e7b55-116">Se você encontrar esse tipo de conflito, resolva-o recriando a migração.</span><span class="sxs-lookup"><span data-stu-id="e7b55-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="e7b55-117">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="e7b55-117">Follow these steps:</span></span>

1. <span data-ttu-id="e7b55-118">Anular a mesclagem e reversão para seu diretório de trabalho antes da mesclagem</span><span class="sxs-lookup"><span data-stu-id="e7b55-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="e7b55-119">Remova sua migração (mas mantenha suas alterações no modelo)</span><span class="sxs-lookup"><span data-stu-id="e7b55-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="e7b55-120">Mescle as alterações de seu colega de equipe em seu diretório de trabalho</span><span class="sxs-lookup"><span data-stu-id="e7b55-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="e7b55-121">Adicione novamente sua migração</span><span class="sxs-lookup"><span data-stu-id="e7b55-121">Re-add your migration</span></span>

<span data-ttu-id="e7b55-122">Depois de fazer isso, as duas migrações podem ser aplicadas na ordem correta.</span><span class="sxs-lookup"><span data-stu-id="e7b55-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="e7b55-123">A migração é aplicada primeiro, renomeando a coluna para *alias*, depois que sua migração a renomeia para *username*.</span><span class="sxs-lookup"><span data-stu-id="e7b55-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="e7b55-124">Sua migração pode ser compartilhada com segurança com o restante da equipe.</span><span class="sxs-lookup"><span data-stu-id="e7b55-124">Your migration can safely be shared with the rest of the team.</span></span>
